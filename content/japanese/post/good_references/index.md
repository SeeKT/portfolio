---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "研究室に入った後今までお世話になっている神サイト/ツールたち"
subtitle: ""
summary: ""
authors: []
tags: [PC, Ubuntu, Docker]
categories: [PC]
date: 2021-09-04T17:26:46+09:00
lastmod: 2021-09-04T17:26:46+09:00
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
研究室に残す予定のものをまとめているときに，色々と参考にしたウェブサイトや記事，使ったツールなどを記録しておこうと思った．


##### 目次
- 1 [Ubuntu関連](#section1)
- 2 [Python関連](#section2)
- 3 [Docker関連](#section3)
- 4 [進捗管理関連](#section4)
- 5 [文献管理関連](#section5)
- 6 [論文関連](#section6)
- 7 [プレゼン関連](#section7)
- 8 [まとめ](#section8)

### 1. <a name="section1">Ubuntu関連</a>
- [金子邦彦研究室](https://www.kkaneko.jp/index.html)
  - OSやアプリケーションのインストールの方法がとても丁寧にまとめられている ([インストール，運用](https://www.kkaneko.jp/tools/index.html))．
  - 個人的には，新しいアプリケーションを使おうとするときに一番苦労するのがインストールなので，このように丁寧にまとめられていてとても助かった．
- [stackoverflow](https://stackoverflow.com/)
  - UbuntuなどのオープンソースのOSを使っているときは，エラーが出た際のトラブルシューティングは自分で何とかしなければならない．
  - 出力されたエラーログを stackoverflow で検索すると，大抵自分と同じエラーが出た人の質問が出てくるので，かなり参考になる．
  - また，プログラムを書いているときに，パッケージの依存関係のエラーが出ることがあるが，このようなときも同様にエラーログを調べられる．
- [Linux Fan Info](https://linuxfan.info/)
  - Ubuntuを使うときにインストール直後にやった方が良いことや，パッケージのインストールの方法などが日本語で説明されているのが良い．
  - 特にUbuntuを使い始めた頃はかなり役立った．


### 2. <a name="section2">Python関連</a>
- [Python 数値計算プログラミング](https://python.atelierkobato.com/)
  - 数値計算のためのパッケージの解説がとても丁寧なのが良い．
  - Matplotlibの解説もあるのが最高．
- [note.mkmk.me Python関連記事まとめ](https://note.nkmk.me/python-post-summary/)
  - Pythonの基本的な使い方，特に組み込み関数の使い方が丁寧にまとめられているのが良い．

### 3. <a name="section3">Docker関連</a>
- [Dockerで環境構築するための最低限の概念理解](https://qiita.com/minato-naka/items/e9cd026747693759800c)
  - Dockerの概念を理解するための助けになった．
  - 図を用いて説明してくれているので，イメージしやすい．
- [いまさらだけどDockerに入門したので分かりやすくまとめてみた](https://qiita.com/gold-kou/items/44860fbda1a34a001fc1)
  - とても丁寧にまとめられている．
  - コマンドの実行例を載せてくれているので，したい処理に対してどのようなコマンドを実行すればよいかわかる．
- [VS CodeでDocker開発コンテナを便利に使おう](https://qiita.com/Yuki_Oshima/items/d3b52c553387685460b0)
  - VSCodeの拡張機能の使い方が書いている．
  - この拡張機能を使うと，コンテナ内の開発をローカルで行っているようにできる (remote sshとかremote wslみたいな感じ) ので，おすすめ．

### 4. <a name="section4">進捗管理関連</a>
- [Git](https://git-scm.com/), [GitHub](https://github.co.jp/)
  - 書いているコードの管理や執筆している論文の管理．
  - 自宅のPCと研究室内のPCでのデータの同期にも使える．
  - VSCodeやAtomの拡張機能を使えばリポジトリの管理が楽．
- [Evernote](https://evernote.com/intl/jp)
  - 私は各週や日のタスクや目標をチェックボックスに書き，やる必要があることと無理してやらなくてもいいことを区別するために使っていた．
  - 複数端末でログインできるのも良い (アプリをダウンロードせずブラウザから入れば端末制限に引っかからない)．

### 5. <a name="section5">文献管理関連</a>
- [Mendeley](https://www.elsevier.com/ja-jp/solutions/mendeley)
  - ある程度の容量を無料で使えるのが強い．
  - 引用のためのbibファイルを作るときに楽．
- [PaperShip](https://www.papershipapp.com/)
  - iPadで論文を読むときに便利．
  - Mendeleyとのアカウント連携ができる．
- [BibTeX entry from URL](https://chrome.google.com/webstore/detail/bibtex-entry-from-url/mgpmgkhhbjgkpnanlmlhibjfgpdpgjec?hl=ja)
  - Chromeの拡張機能で，URLからbibtexの形式を取得できるもの．
  - 記事やウェブサイトを引用するときに重宝する．

### 6. <a name="section6">論文関連</a>
- [minoblog](https://ocoshite.me/)
  - 研究の方向性に迷っていたときにこのブログを読み，非常に感銘を受けた．
  - 特に以下の2つの記事はおすすめ．
    - [【超重要】研究室に配属された学生が最初に学ぶべき論文の読み方【4つのポイント】](https://ocoshite.me/how-to-read-scientific-articles)
    - [【なぜ論文を読むのか？】研究におけるメリットと重要性【アイデアは知識から】](https://ocoshite.me/the-importance-and-benefits-of-reading-scientific-articles)
- [Seitaro Shinagawa, 2020.06.01 M1勉強会 論文の読み方・書き方・研究室の過ごし方](https://speakerdeck.com/sei88888/2020-dot-06-dot-01-m1mian-qiang-hui-lun-wen-falsedu-mifang-shu-kifang-yan-jiu-shi-falseguo-gosifang)
  - 論文の読み方に関しては，特に「速読」と「精読」の部分が素晴らしいと思った．本当に参考になった．
  - 論文の書き方もとても参考になる．
- [参考文献の書き方](http://yoshitakaushiku.net/how_to_write_references.html)
  - 恥ずかしながら，私はM1まで教授に参考文献の書き方を直されていた． 
  - このサイトは，参考文献の書き方のルールがまとめられていて，論文を執筆するときの参考になった．
- [Grammarly](https://app.grammarly.com/)
  - Googleアカウントがあれば使うことができる優秀なスペルチェッカー．
  - 自分で書いているときにはなかなか気付けないスペルや文法のミスを指摘してくれるので，論文執筆時に重宝する．
  - 専門用語もミスとして指摘することがあるが，それ以外は素晴らしいツールだと思う．
- [DeepL](https://www.deepl.com/ja/translator)
  - 論文執筆時に英語の表現が思いつかないときや，論文を読んでいるときに意味がつかめないときにかなり使える．
  - 精度が良い．
  - 論文をDeepLを使って書くことについて色々言われているが，全部DeepLを使うのではなく，どうしても思いつかない表現で使うというような使い方をすれば良いのではないだろうか．
  - Grammarlyと組み合わせるとある程度マシな英語が書けると思う．

特に上2つのような素晴らしいブログやスライドを見ると，自分が底辺に思えてきます．

### 7. <a name="section7">プレゼン関連</a>
- [研究発表のためのプレゼンテーション技術](https://www.slideshare.net/ShinnosukeTakamichi/ss-48987441)
  - プレゼンを構成する要素が分かりやすく書かれている．
  - 私はこの記事を参考にスライドの構成を考えている．
- [わかりやすい研究発表をするための3つの手順【スライド・話し方】](https://ocoshite.me/how-to-make-a-good-presentation)
  - minonさんの記事．
  - スライドの作り方の他，発表の仕方などもとても参考になる．
- [OBS Studio](https://obsproject.com/ja/download)
  - 最近はオンライン学会が増えてきたり，学会発表がビデオ投稿のみであるものが多くなったりしている．また，学会発表の練習のために，自分の発表を録音して聞くことが有効だと考えられる (これはminonさんの記事でも挙げられている)．
  - 発表の録画に役立つのがOBS Studioである．録画が非常に容易に行えるのが良い．

### 8. <a name="section8">まとめ</a>
この記事では，研究室に入った後から今までお世話になっている神のウェブサイトや記事，ツールをまとめた．他にも良いものがある (見つける) と思うので，気付いたらこの記事も更新していきたいと思う．