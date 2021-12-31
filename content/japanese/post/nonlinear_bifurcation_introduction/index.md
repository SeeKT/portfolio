---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "力学系の分岐現象: 導入"
subtitle: ""
summary: ""
authors: []
tags: [Nonlinear systems, Python, C++]
categories: [Nonlinear Systems]
date: 2021-08-28T12:26:25+09:00
lastmod: 2021-09-04T16:26:25+09:00
featured: false
draft: true

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
私は研究で非線形系の分岐現象を扱っている．私が修士の間に勉強してまとめていた非線形力学系の分岐現象 (特に1次元の場合) の基本的なことをブログに書きたいと思う．

私が勉強したことは研究に必要だった基礎的なことだけなので，それ以上のことは理解していないので，まとめ終わったらもっと勉強したいと思う．

### 1. 今後の予定といくつかの文献の紹介
M1の冬からM2の春にかけて文献[^1]を参考にして非線形力学系の分岐現象についてまとめ，プログラムを書いていた．これまでまとめた資料とプログラムをブログの記事で公開したいと思っている．

[^1]: 小室，[基礎からの力学系](https://www.saiensu.co.jp/book_support/sgc-17/)，サイエンス社，2002.

非線形力学系については多くの優れた文献がある．例えば，文献[^2]は，非線形システムの制御の教科書の名著として知られている．非線形システムの安定性の解析について分かりやすくまとめられていると思う．

[^2]: H. K. Khalil, [Nonlinear Systems](https://www.egr.msu.edu/~khalil/NonlinearControl/), 3rd ed., Prentice Hall, 2002.

また，文献[^3]は力学系とカオスの入門書の名著として知られている．日本語の訳書も出ており，直感的な説明も数学的な解説もあるのが良い．私はこの文献は辞書代わりに使っていたが，時間があり気が向いたらしっかり読んでみたいと思っている．

[^3]: S. H. Strogatz, [Nonlinear Dynamics and Chaos: With Applications To Physics, Biology, Chemistry, And Engineering](http://www.stevenstrogatz.com/books/nonlinear-dynamics-and-chaos-with-applications-to-physics-biology-chemistry-and-engineering), CRC press, 2001.

文献[^4]は分岐現象の条件の直感的な説明と証明が記述されており，論文を執筆するときに参考にした．

[^4]: S. Wiggins, [Introduction to Applied Nonlinear Dynamical Systems and Chaos](https://www.researchgate.net/publication/258629276_Introduction_To_Applied_Nonlinear_Dynamical_Systems_And_Chaos), 2nd ed., Springer, 2003.

他にも名著はたくさんあるが，私が修士の間に勉強に使ったのは主にこの4つの本である．特に分岐現象の勉強には，文献[^1]を用いた．