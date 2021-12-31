---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Ubuntu20.04にDocker Engineをインストールする"
subtitle: ""
summary: ""
authors: []
tags: [Docker, Ubuntu]
categories: [Ubuntu]
date: 2021-07-08T18:25:39+09:00
lastmod: 2021-08-30T09:28:39+09:00
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
私が学部4年から修士1年までに使っていたPythonの開発環境はUbuntu上にpipとpipenvを入れ，pipenvで仮想環境を作って使うというものだった．Ubuntuを20にアップグレードしたときにこれまで使っていた仮想環境でプログラムが動かない，ということが起こった．

このように，OSに1つの環境を作って動かそうとした際には，更新に伴いプログラムが動かなくなるというリスクがある．さらに，以下のような問題が挙げられる．
- OSが汚れる
  - OSに直接インストールするので，複数のバージョンが共存するといったことが起こる．
- 問題の切り分けが難しい
  - OSのアップグレードの問題なのか，パッケージの問題なのか分からなくなる．

そんなときDockerに出会う．Dockerの説明は省略するが，イメージは1つのサーバに独立した複数のサーバを同時に構築することができる，というものである．これは良いと思い，研究の開発環境をDocker container上に作ることにした．

Ubuntu20.04にDocker Engineをインストールするためのシェルスクリプトを作ったので，この記事で共有する．

### 1. Dockerの利点と欠点
私が思うDockerの利点と欠点を以下に示す．あまり詳しくないエアプ発言かもしれない．

#### 利点
- Dockerfileに開発に必要なパッケージのインストールについて記述できるので，Dockerfileとして記述してしまえば，そのとおりに環境構築ができる．
- 移植しやすい．
- Python等のバージョンが変わったときもDockerfileの内容を変えることで最新版にアップデートできる．
- OSのバージョンに依らない．依存関係に関する問題を，パッケージのバージョンにまで絞り込める．
- OSが汚れない．
- 1つのPCの中に複数の異なる開発環境を構築できる．
- VSCodeの拡張機能が優秀で，使いやすい．
#### 欠点
- Windowsでエラーが出ることが多い気がする．Windowsで同じようなことをしようと思ってもうまく動かないことがある．
- WindowsではWSL2をバックエンドにしているので，メモリの消費量が多い．まともに動かそうと思うとメモリ16GBはないと厳しい．
- 導入コスト．(自分が賢くないだけだけど) 難しい．

### 2. インストールのためのシェルスクリプト
福山大学の[金子邦彦先生のウェブサイト](https://www.kkaneko.jp/tools/docker/ubuntu_docker.html) がかなり参考になった．Docker Engineの導入以外にもかなり参考にさせていただいた．

次のようなシェルスクリプトを作り，```~/install_docker.sh```として保存した．
```shell
#!/bin/bash

# Written by A.Tachibana, 2021/3/19

# Objective
## To install docker in Ubuntu 20

# Reference
## https://www.kkaneko.jp/tools/docker/ubuntu_docker.html

############ To execute ############
# chmod 755 install_docker.sh
# ./install_docker.sh
####################################


# Require password
printf "password: "
read -s password

# Update and Upgrade
echo "$password" | sudo -S apt update && sudo -S apt -y upgrade

# Install Docker
## Delete old version (if exists)
sudo -S apt -y remove docker docker-engine docker.io containerd docker-ce docker-ce-cli
sudo -S apt -y autoremove
## Install required software
sudo -S apt update
sudo -S apt -y install apt-transport-https ca-certificates curl software-properties-common
sudo -S apt -y install linux-image-generic
## Set docker repository
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
apt-key fingerprint 0EBFCD88
sudo -S add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -sc) \
    stable"
sudo -S apt update
## Install docker.io
sudo -S apt -y install docker.io containerd docker-compose
## Add authority
sudo -S usermod -aG docker $USER
## Set autostart
sudo -S systemctl unmask docker.service
sudo -S systemctl enable docker
sudo -S systemctl is-enabled docker
## let user ubuntu use docker
sudo gpasswd -a $USER docker
```

このファイルに実行権限を与え，実行する．

```bash
chmod 755 install_docker.sh
./install_docker.sh
```

なお，このシェルスクリプトは，私のサブのGitHubアカウントの[publicリポジトリ](https://github.com/tcbn-ai/TIL/blob/main/Study_Docker/shellscript/install_docker.sh)にアップロードしている．

#### 参考
- [Docker Engine のインストールと使用法（Ubuntu 上）](https://www.kkaneko.jp/cc/vm/ubuntu_docker.html)