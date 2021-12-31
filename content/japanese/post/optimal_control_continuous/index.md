---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "最適制御問題: 連続時間システムの最適制御"
subtitle: ""
summary: ""
authors: []
tags: [Optimal control]
categories: [Optimal control]
date: 2021-07-22T16:14:47+09:00
lastmod: 2021-07-22T16:14:47+09:00
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
先日は，4章の内容をまとめた ([最適制御問題: 変分法](/post/optimal_control_validation/))．今回は，5章の内容をまとめる．

### 1. 5章の概要
5章では，連続時間システムに対する最適制御問題を扱っている．最適制御問題の基本的な問題設定，変分法から導出した停留条件 (オイラー・ラグランジュ方程式)，局所最適性の十分条件，最適解の摂動についてまとめられている．このブログではオイラー・ラグランジュ方程式までをまとめる (詳しい導出は省略)．詳しい導出とそれ以降の内容は手書きの資料に載せている．

ここで扱う最適制御問題は連続時間システム

$$\dot{x}(t) = f(x(t), u(t), t) \tag{5.1}$$

(ただし，$$x(t) \in \mathbb{R}^n$$は状態ベクトル，$$u(t) \in \mathbb{R}^m$$は制御入力ベクトル) に対して，初期時刻$$t_0$$，終端時刻$$t_f$$，初期状態$$x(t_0) = x_0$$が与えられた下で評価関数

$$J = \varphi(x(t_f)) + \int_{t_0}^{t_f} L(x(t), u(t), t) dt \tag{5.2}$$

が与えられ，それを最小化するような最適制御$$u(t)$$を求める，というような問題である．つまり，ここで考える最適制御問題は，関数$$x(t)$$と$$u(t)$$の汎関数である評価関数$$J$$を，等式制約である状態方程式の下で最小化する変分問題である．

等式制約

$$f(x, u, t) - \dot{x} = 0$$

に対応するラグランジュ乗数のベクトルを$$\lambda(t) \in \mathbb{R}^n$$として，制約条件の下での停留条件を求めるための汎関数$$\overline{J}$$を構成すると，

$$\overline{J} = \varphi(x(t_f)) + \int_{t_0}^{t_f} \{L(x, u, t + \lambda^{\mathrm{T}} (f - \dot{x}) \}dt \tag{5.3}$$

となる．ここで，スカラー値関数$$H$$を

$$H(x, u, \lambda, t) := L(x, u, t) + \lambda^{\mathrm{T}} f(x, u, t)$$

と定義する．$$H$$は最適制御問題の**ハミルトン関数**と呼ばれる．この$$H$$を用いると，汎関数$$\overline{J}$$は，以下のように$$\dot{x}$$の項とそれ以外の項に分けて書き直される．

$$\overline{J} = \varphi(x(t_f)) + \int_{t_0}^{t_f} \left(H(x, u, \lambda, t) - \lambda^{\mathrm{T}}\dot{x}\right)  dt$$

上記の問題設定の下で，評価関数 (5.2) を最小にする最適制御 $$u(t) \ (t_0 \leq t \leq t_f)$$ が存在するとし，対応する最適軌道を$$x(t)$$とすると，$$n$$次元ベクトル値関数$$\lambda(t)$$が存在して以下の**オイラー・ラグランジュ方程式**が成り立つ．

$$\dot{x} = f(x, u, t), \ x(t_0) = x_0 \tag{5.4}$$

$$\dot{\lambda} = - \left(\frac{\partial H}{\partial x} \right)^{\mathrm{T}}(x, u, \lambda, t), \ \lambda(t_f) = \left( \frac{\partial \varphi}{\partial x} \right)(x(t_f)) \tag{5.5}$$

$$\frac{\partial H}{\partial u}(x, u, \lambda, t) = 0 \tag{5.6}$$

これは，$$x(t)$$と$$u(t)$$の連立微分方程式とみなすことができるが，$$x(t)$$は初期状態$$x(t_0)$$が与えられているのに対し，$$\lambda(t)$$は終端値$$\lambda(t_f)$$に対する条件が与えられている．このような問題を**2点境界値問題**という．多くの場合，非線形の微分方程式の解析解は得られないので，初期状態を未知のパラメータとして終端条件が成り立つための条件を書き下すことは困難である．

以下に，各節の内容をまとめた手書きのメモを掲載する．

- [5.1節](/files/optimal_control/Sec5.1.pdf)
- [5.2節](/files/optimal_control/Sec5.2.pdf)
- [5.3節](/files/optimal_control/Sec5.3.pdf)
- [5.4節](/files/optimal_control/Sec5.4.pdf)


### 2. まとめ
ここまで読んで，最適制御問題は状態と入力の汎関数である評価関数を等式制約である状態方程式の下で最小化する変分問題であり，最適制御と最適軌道，ラグランジュ乗数に対してオイラー・ラグランジュ方程式が成り立つことは分かったが，その後どうするのかまだ分かっていない．連休中にやる気があれば6章と7章を読みたいと思っている．