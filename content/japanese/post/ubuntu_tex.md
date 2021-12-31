---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Ubuntu20.04にTeX Liveをインストールする"
subtitle: ""
summary: ""
authors: []
tags: [TeX, Ubuntu]
categories: [Ubuntu]
date: 2021-07-08T18:00:11+09:00
lastmod: 2021-07-08T18:00:11+09:00
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
- UbuntuにTeXを入れたときのメモ
### 1. やること
0. 好きなエディタを入れる
1. TeXLiveを入れる
2. ~/.latexmkrcを作る
3. 動作テスト
#### 好きなエディタを入れる
- 私はTeXの文書を作るときはAtomを使うので，Atomを入れた．
    - 公式サイト (https://atom.io/) からdebをダウンロードして実行．
        ```
        sudo apt install ./atom-amd64.deb
        ```
    - パッケージを入れる．
        - latex
        - language-latex
        - latexer
        - pdf-view
#### TeXLiveを入れる
- 日本の大学(e.g. NAIST)のリポジトリからダウンロードして中身を展開する．
    ```
    wget http://ftp.naist.jp/pub/CTAN/systems/texlive/tlnet/install-tl-unx.tar.gz
    tar -zxvf install-tl-unx.tar.gz
    cd install-tl-[date]
    ```
- 管理者権限で実行する．リポジトリはダウンロードしたところ．
    ```
    sudo ./install-tl --repository http://ftp.naist.jp/pub/CTAN/systems/texlive/tlnet/
    ```
    - "I"でインストール
- パスを通す．
    ```
    sudo /usr/local/texlive/2020/bin/x86_64-linux/tlmgr path add
    ```
    - これは私の例である．
- TeXLiveのアップデート
    - 参照するリポジトリの指定
        ```
        sudo tlmgr option repository http://ftp.naist.jp/pub/CTAN/systems/texlive/tlnet/
        ```
    - アップデート
        ```
        sudo tlmgr update --self --all
        ```

#### ~/.latexmkrcの作成
- 毎回コンパイル(platexしてdvipdfmxして...)というのは面倒 → latexmkを使うことで，保存する度に自動的にpdfまで生成するようにする．
    - 参考: [latexmkの薦め](https://qiita.com/tdrk/items/16f31e45826c57bce412)
    - Ubuntuでは以下のファイルをホームディレクトリ直下(~/.latexmkrc)に配置したところ，正常に動作した．
    ```perl
    #!/usr/bin/perl
    $latex = 'platex -guess-input-enc -src-specials -interaction=nonstopmode -synctex=1';
    $latex_silent = 'platex -interaction=batchmode';
    $dvips = 'dvips';
    $bibtex = 'pbibtex';
    $makeindex = 'mendex -r -c -s jind.ist';
    $dvi_previewer = 'start dviout';
    $dvipdf = 'dvipdfmx %O -o %D %S';
    $pdf_previewer = 'xdg-open';
    $preview_continuous_mode = 1;
    $pdf_mode = 3;
    $pdf_update_method = 4;
    ```
    - latexmkについては，
        ```bash
        vim ~/.latexmkrc
        ```
        などとして，上記のようなものを作ればよい．

#### 動作テスト
##### platex
- "test.tex"という名前のファイルをtextestというディレクトリの中に作った．
    ```tex
    \documentclass[dvipdfmx]{jsarticle}
    \title{ {\LaTeX} 動作テスト・サンプルファイル}
    \date{\today}
    \begin{document}
    \maketitle
    \section{test}
    これはテストです．
    \begin{equation}
      f(x) = 2x + 3
    \end{equation}
    \end{document}
    ```
- textestというディレクトリに移動して，以下のコマンドを実行する．
    ```bash
    platex test.tex && dvipdfmx test.dvi
    ```
    実行後にpdfファイルが問題なく生成されていたら良い．
##### latexmk
- 先程のファイルに対して，
    ```bash
    latexmk test.tex
    ```
    として，正しくpdfファイルが生成され，変更して保存した後にpdfファイルが更新されたら問題なく導入されている．
