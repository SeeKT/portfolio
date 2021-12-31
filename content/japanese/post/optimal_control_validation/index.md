---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "最適制御問題: 変分法"
subtitle: ""
summary: ""
authors: []
tags: [Optimal control]
categories: [Optimal control]
date: 2021-07-18T16:18:58+09:00
lastmod: 2021-07-19T11:15:58+09:00
featured: false
draft: false
markup: mmark

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
研究で最適制御問題を扱うことになったが，これまでまともに勉強してこなかったので，勉強する．研究では連続時間の最適制御問題を扱う予定なので，連続時間の最適制御問題の文献を読んでまとめた手書きのメモを公開することにした．

次の本を参考にしている．4章から7章まで勉強する予定である．
- [大塚，非線形最適制御問題入門，コロナ社，2011](https://www.coronasha.co.jp/np/isbn/9784339033182/)

#### 0.1. 方針
GoodNotesを使ってメモを取りながら，本を読んで数式を追っている．ある程度まとめたらメモを載せることにする．誤字や記述が怪しい箇所，理解が足りていない箇所も多いと思う．

普段，定理や補題の主張を読み，証明は読み飛ばすか流れだけを見ることが多いので，今回もそうなると思う．

### 1. 4章の概要とまとめ
4章では，変分法の基本がまとめられている．最適制御問題は，時間の関数である状態 $$x(t)$$ と入力 $$u(t)$$ の **汎関数** である評価関数 $$J$$ を，**等式制約** (状態方程式) の下で最小化する問題であり，これは変分問題である．よって，変分法は最適制御問題の基礎になっている．

以下に，各節の内容をまとめた手書きのメモを掲載する．

- [4.1節](/files/optimal_control/Sec4.1.pdf)
- [4.2節](/files/optimal_control/Sec4.2.pdf)
- [4.3節](/files/optimal_control/Sec4.3.pdf)
- [4.4節](/files/optimal_control/Sec4.4.pdf)
