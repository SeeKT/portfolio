---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "最適制御問題: 動的計画法と最小原理"
subtitle: ""
summary: ""
authors: []
tags: [Optimal control]
categories: [Optimal control]
date: 2021-08-02T21:12:28+09:00
lastmod: 2021-08-02T21:12:28+09:00
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
先日は，5章の内容をまとめた ([最適制御問題: 連続時間システムの最適制御](/post/optimal_control_continuous/))．少し間が空いてしまったが，今回は6章の内容をまとめる．

### 1. 6章の概要
6章では，最適制御問題に動的計画法を適用して，HJB方程式という偏微分方程式を導出している．変分法で導かれた常微分方程式の2点境界値問題であるオイラー・ラグランジュ方程式と，動的計画法から導かれた偏微分方程式であるHJB方程式は何らかのつながりがある．動的計画法から最小原理と呼ばれる条件を経由してオイラー・ラグランジュ方程式が導かれる．

以下に，各節の内容をまとめた手書きのメモを掲載する．

- [6.1節](/files/optimal_control/Sec6.1.pdf)
- [6.2節](/files/optimal_control/Sec6.2.pdf)
- [6.3節](/files/optimal_control/Sec6.3.pdf)

### 2. まとめ
ここまで読んで，オイラー・ラグランジュ方程式とHJB方程式の関係が何となく分かってきた．実際の数値計算をどうするか，ということで少し詰まってしまって時間がかかっている．