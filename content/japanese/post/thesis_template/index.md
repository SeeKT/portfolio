---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "卒論・修論のテンプレート (TeX) について"
subtitle: ""
summary: ""
authors: []
tags: [TeX]
categories: [PC]
date: 2021-10-09T17:37:18+09:00
lastmod: 2021-10-09T17:37:18+09:00
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
研究室単位で修論のテンプレートがあると思う．私の研究室にあったテンプレートはTeXソースの分割がされていないようなものだったので，テンプレートを作り直すついでに備忘録として残しておくことにする．

作ったテンプレートと同様のものは，GitHubリポジトリ [SeeKT/thesis_template](https://github.com/SeeKT/thesis_template) にアップロードした．自分が使うためのテンプレートなので色々と問題点もあるだろうが，ご容赦願いたい．

### 1. ディレクトリ構成
卒論や修論は文章の量が多くなるので，1つのTeXソースに全て書こうとするとTeXソースの行数が増え，修正が大変になる．そこで，修論本体のTeXソースに各章のTeXソースをinputするようにする．

今回は，以下のようなディレクトリ構成にする．

```bash
|- bib/: bibtexするもの
|   |- hogehoge.bib
|
|- fig/: 図を入れるディレクトリ
|
|- main/: main.tex のディレクトリ
|
|- tex/: main.texにinputするTeXソースのディレクトリ
|   |- preamble_thesis.tex: プリアンブルをまとめたTeXファイル
|   |- その他inputするTeXソース
```

### 2. コンパイル方法等
今回は，documentclass を `jsbook` としている．コンパイルは `platex` 等で行うと良いように思う．

[Ubuntu20.04にTeX Liveをインストールする](https://tachibana-ai.netlify.app/post/ubuntu_tex/) にまとめた`latexmk` を使うと，`bibtex` あたりの処理が楽になる．

コンパイル時は，例えば

```bash
latexmk main.tex
```

とする．

<details>
<summary>卒論/修論本体のTeXソース</summary>

```tex
% 卒論/修論 テンプレート
\documentclass[a4j,report,dvipdfmx]{jsbook}
% 英語のときは
% \documentclass[a4j,report,english]{jsbook}

% preamble_thesis.tex を input
\input{../tex/preamble_thesis}
\begin{document}

%%%%%%%%%%%% Title %%%%%%%%%%%%
\input{../tex/title}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%% Table of Contents %%%%%%%%%%%%
\clearpage
\tableofcontents
\clearpage
\setcounter{page}{0}
\pagenumbering{arabic}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%% Chapter1 Introduction %%%%%%%%%%%%
\input{../tex/1_Intro}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%% Chapter2 Preliminaries %%%%%%%%%%%%
\input{../tex/2_preliminaries}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%% Chapter3 main1 %%%%%%%%%%%%
\input{../tex/3_main1}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%% Chapter4 main2 %%%%%%%%%%%%
\input{../tex/4_main2}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%% Chapter5 Conclusion %%%%%%%%%%%%
\input{../tex/5_conclusion}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%% Acknowledgement %%%%%%%%%%%%
\input{../tex/acknowledgement}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%% References %%%%%%%%%%%%
% 参考文献のスタイルファイル (hogehoge.bst)
\bibliographystyle{junsrt}
% 参考文献のbibtexファイル (main.texからの相対パスで指定, hogehoge.bib)
\bibliography{../bib/ref_circle,../bib/ref_triangle}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\end{document}
```
</details>

#### (個人的に思う) 利点
- 卒論や修論が長くなっても，本体の長さはそれほど変わらない．
- 修正の際は各章のTeXソースを修正すれば良い．

### 3. GitHubリポジトリ
GitHubリポジトリとして卒論や修論を管理するのが推奨されているように思う．ここでは，このリポジトリの `.gitignore` を紹介する．

```txt
*.aux
*.fls
*.log
*.fdb_latexmk
*.toc
*.synctex.gz
*.dvi
```

TeXのコンパイルでは様々な副産物ができるが，提出時に必要なものはTeXソースとPDFファイルのみであることが多い．[SeeKT/thesis_template](https://github.com/SeeKT/thesis_template) では副産物をGitの管理に含めないようにしている．

### 4. まとめ
この記事では，作成した卒論/修論のテンプレートについて簡単に紹介した．私はこのテンプレートを卒論/修論だけでなく長めのTeXの資料 (`jsbook`) を作るときにも活用している．