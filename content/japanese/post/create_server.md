---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "ファイルサーバを構築したときのログ (Ubuntu18.04)"
subtitle: ""
summary: ""
authors: []
tags: [Ubuntu, Server]
categories: [Ubuntu]
date: 2021-07-09T18:39:46+09:00
lastmod: 2021-07-09T18:39:46+09:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

### 0. はじめに
以前ファイルサーバを構築したので，そのときのログを残す．

色々と怪しい部分もあるかもしれないので，一個人の記録として見ていただければ幸いである．

以下，LinuxサーバをUbuntu18.04とし，クライアント側はWindows10もしくはUbuntu20.04とする．Windows10の場合はGit Bashが導入済みであるものとする．

1. SSH接続の設定
2. Sambaの設定
3. バックアップの設定
4. CUI運用にする

という流れで実行した．


### 1 SSH接続
サーバにリモートで接続して手元で扱うときには，SSH (Secure Shell) 接続を用いることが多い．SSHは，セキュアな通信でリモートアクセスするためのプロトコルであり，強固な認証と暗号化の機能がある (詳しい説明は省略)．ここでは，LinuxサーバのSSH設定の手順について述べる．認証方式は公開鍵認証とする．

以下，サーバ側のシェルに```(server)```とし，接続するクライアント側のシェルに```(client)```と書くものとする．

#### (1) sshdのインストールと自動起動の設定 (サーバ側)
ホスト側でsshをインストールして，自動起動の設定を行う．
```bash
(server) sudo apt update && sudo apt -y upgrade
(server) sudo apt -y install ssh
(server) sudo systemctl start ssh.service
(server) sudo systemctl enable ssh.service
```

この後サーバを再起動し，以下のコマンドを実行．
```bash
(server) sudo systemctl is-enabled ssh.service
(server) sudo systemctl status ssh.service
```

上のコマンドを実行し，```enabled```と出力されれば，ブート時に起動する．下のコマンドを実行し，```Active: active (running)```と出力されれば現在sshdが起動している．

この時点でサーバ側にパスワード認証で入ることができる．
```bash
(client) ssh [user]@[IP address of the server]
```
(userはサーバのユーザ名)とすると，特に設定が間違えていなければ接続できる．

今回の目的は公開鍵認証で入ることなので，以下の手順を

#### (2) キーペアを作る (クライアント側)
クライアント側でキーペアを作る．キーペアの作り方については以下の記事を参考にした．

Git Bash (Ubuntuはterminal)で，以下のコマンドを実行する．
```bash
(client) ssh-keygen -t rsa -b 4096 -C "comment"
```

上記のコマンドを実行すると，鍵のパスを変えるか，パスフレーズは必要か聞かれる．鍵はデフォルトでは```<user>/.ssh```直下に```id_rsa```と```id_rsa.pub```として保存されるが，複数の鍵を作る場合は新しいディレクトリを```.ssh```以下に作るなどすれば良いと思う．

commentは任意だが，複数人が管理するファイルサーバという機能を持たせると考えると，自分の名字にするのが良いように思う．

##### 参考
- [お前らのSSH Keysの作り方は間違っている](https://qiita.com/suthio/items/2760e4cff0e185fe2db9)


#### (3) クライアント側からホスト側に公開鍵を送信する
(1)でSSH接続ができているとすると，クライアント側でscpコマンドを実行すると，サーバ側のuserのホームディレクトリ直下に公開鍵を転送できる．
```bash
(client) scp [client rsa pub key path] [user]@[IP address of the server]:/home/[user]
```
ここで，scpはSSHを使ってリモートホストとローカルホストの通信を暗号化した上で，ファイルの送信をするコマンドであり，```scp [from] [to]```のように書けば，fromのパスで指定したファイルがtoのパスの下に送られる．

```[client rsa pub key path]```は，キーペアを作るときに変えていなければ，(Windowsの場合は)
```cmd
C:\Users\[user]\.ssh\id_rsa.pub
```
である．

#### (4) ```authorized_keys```ファイルを作る
次に，サーバ側で鍵の設定をする．(3)で，ホームディレクトリ直下に公開鍵が送られたとする．このファイルの内容を```authorized_keys```ファイルにコピーする．その後，```chmod```コマンドで所有者のみが読めるようにする．
```bash
(server) cat id_rsa.pub >> ./.ssh/authorized_keys
(server) chmod 700 ./.ssh
(server) chmod 600 ./.ssh/authorized_keys
(server) rm -rf id_rsa.pub
```

#### (5) ```sshd_config```の設定 (サーバ側)
公開鍵認証を有効化するために，sshdの設定ファイルを編集する．一旦バックアップをとる．
```bash
(server) cd /etc/ssh
(server) sudo cp sshd_config sshd_config.bk
(server) sudo vim sshd_config
```

以下の行のコメントを外し，有効化する．
```txt
#PubkeyAuthentication yes
#AuthorizedKeysFile .ssh/authorized_keys  .ssh/authorized_keys2
```
一応Rootでのログインはできないようにしておく．
```txt
PermitRootLogin no
```

##### 参考
- [SSHの鍵認証設定](https://qiita.com/gotohiro55/items/36a22516de2b381b3c6e)

#### (6) ```.ssh```のconfigファイルの設定 (クライアント側)
(5)までで公開鍵認証でSSH接続できるようになった．
```cmd
ssh -i [client rsa private key path] [user]@[IP address of the server]
```

接続のたびにこのコマンドを打つのはめんどくさいので，エイリアス設定をする．```.ssh/config```ファイルに以下を追加 (Windowsの場合．秘密鍵が```.ssh```直下にあるとする)．
```txt
Host [server name]
  HostName xxx.xxx.xxx.xxx  # IP address of the server
  User [user]               # user name of the server
  Identityfile C:\Users\[user]\.ssh\id_rsa
```
このようにすると，
```
ssh [servername]
```
でサーバに接続できる．


### 2 Sambaの設定
Sambaとは，Linux上でWindowsのネットワーク機能を実現するソフトウェアである．これを使うことで，ファイルサーバの機能が実現される．

以下，ホームディレクトリ直下の```Share_dir```を共有するものとする．ここでは，同一のネットワークのホストに対してフルパーミッションで権限を与える場合を想定する．

##### 参考
- [【Linux】Ubuntuでファイルサーバーを作って遊ぼう！(中級者～上級者向け)【世界一わかりやすい解説(かもしれない)】](https://www.youtube.com/watch?v=eVVfJYXq1ug&list=LL&index=1&t=740s)
  - このYouTubeの動画を参考にした．

#### (1) Sambaのインストール
```bash
(server) sudo apt install -y samba
```

#### (2) ```samba.conf```のバックアップと編集
```bash
(server) cd /etc/samba/
(server) sudo cp smb.conf smb.conf.bk
(server) sudo vim smb.conf
```

末尾に以下を追加．
```txt
# fileserver
[fileserver]
comment = Ubuntu FileServer
path = /home/[user]/Share_dir/
browseable = yes
read only = no
guest ok = yes
guest only = yes
create mode = 0777
directory mode = 0777
```

#### (3) 再起動と自動起動の設定
```bash
(server) sudo systemctl restart smbd nmbd
(server) sudo systemctl enable smbd nmbd
```

### 3. バックアップ
共有したディレクトリをHDDなどにバックアップすることを考える．これは，突然PCが落ちてデータが飛ぶようなリスクへの対策となる．

ここではHDDがマウントポイント```/mnt/Elements```にマウントされているとし，システム起動時にマウントするように設定したとする．

#### (1) バックアップ用のディレクトリの作成
```Server_backup```ディレクトリにバックアップを取るものとする．
```bash
(server) mkdir /mnt/Elements/Server_backup
```

#### (2) バックアップの設定
ここでは，差分バックアップを取るものとする．このときに使うコマンドは```rsync```である．
```bash
(server) sudo rsync -av /home/[user]/Share_dir/ /mnt/Elements/Server_backup
```
これは，```Share_dir```の中身を```/mnt/Elements/Server_backup```にバックアップするというようなものである．ここで，引数```-av```は，今どこをコピーしているのかを表示出力するためのものである．

#### (3) バックアップのスケジューラの設定
毎日バックアップを更新するために，スケジューラを用いる．crontabを編集すれば良い．

```bash
(server) sudo vim /etc/crontab
```

以下を追加した．
```bash
# m h dom mon dow user	command
00 3	* * *	root    rsync -av /home/[user]/Share_dir/ /mnt/Elements/Server_backup
```

### 4. CUI運用にする
ファイルサーバとしての運用であれば，CUI運用の方が良い．
```bash
# 一時的にCUIモードにする
(server) sudo systemctl isolate multi-user.target
# デフォルトをCUIモードにする
(server) sudo systemctl set-default multi-user.target
```

GUIに戻したいときは以下のコマンドを実行．
```bash
# 一時的にGUIモードにする
(server) sudo systemctl isolate graphical.target
# デフォルトをGUIモードにする
(server) sudo systemctl set-default graphical.target
```

### 5. まとめ
ファイルサーバを作った際のログを残した．