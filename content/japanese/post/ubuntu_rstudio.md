---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Ubuntu20.04にRとRStudioをインストールする"
subtitle: ""
summary: ""
authors: []
tags: [R, Ubuntu]
categories: [Ubuntu]
date: 2021-07-08T18:07:46+09:00
lastmod: 2021-07-08T18:07:46+09:00
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
授業でRとRStudioを使う必要があったのでインストールした．
前処理がめんどくさかったので，シェルスクリプトを書いた．

##### 参考
- [RとRStudioのインストールと初期設定 Linux (Ubuntu) 編](https://yukiyanai.github.io/jp/resources/docs/install-R_ubuntu.pdf)

### 1. 前処理
次のようなシェルスクリプトを書き，```~/install_R_requirement.sh```として保存した．

```shell
#!/bin/bash

# Written by A.Tachibana, 2021/4/28

# Objective
## To install R in Ubuntu 20
## Requirement packages

# Reference
## https://yukiyanai.github.io/jp/resources/docs/install-R_ubuntu.pdf

############ To execute ############
# chmod 755 install_R_requirement.sh
# ./install_R_requirement.sh
####################################

# Require password
printf "password: "
read -s password

# Update and Upgrade
echo "$password" | sudo -S apt update && sudo -S apt -y upgrade

# 1. Install tools
sudo -S apt -y install gdebi-core wget 

# 2. Get fonts
sudo -S apt -y install fonts-ipaexfont
fc-cache -f -v

# 3. Install requirement tools
## requirement tools
sudo -S apt -y install build-essential libxml2-dev libssl-dev libx11-dev libglu1-mesa-dev libmagick++-dev libudunits2-0 libudunits2-dev libgdal-dev libproj-dev libgmp3-dev curl 
```

このファイルに実行権限を与え，実行する．

```bash
chmod 755 install_R_requirement.sh
./install_R_requirement.sh
```

### 2. RとRStudioのインストール
次のようなシェルスクリプトを書き，```~/install_R.sh```として保存した．

```shell
#!/bin/bash

# Written by A.Tachibana, 2021/4/28

# Objective
## To install R in Ubuntu 20

# Reference
## https://yukiyanai.github.io/jp/resources/docs/install-R_ubuntu.pdf

############ To execute ############
# chmod 755 install_R.sh
# ./install_R.sh
    # execute after install_R_requirement.sh
####################################

# Require password
printf "password: "
read -s password

# 1. add repository
sudo -S apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo -S add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'
# 2. install R base
sudo -S apt update
sudo -S apt -y install r-base 
# 3. install RStudio
cd ~/Downloads/
# 2021/4/28
wget https://download1.rstudio.org/desktop/bionic/amd64/rstudio-1.4.1106-amd64.deb
sudo -S gdebi rstudio-1.4.1106-amd64.deb
```

このファイルに実行権限を与え，実行する．
```bash
chmod 755 install_R.sh
./install_R.sh
```

#### 注意点
- RStudioに関しては2021/4/28時点で最新のものをダウンロードしたが，現在はもっと新しいものが出ていると思う．
  - 「最新版を持ってくる」ような書き方が分からなくて，頭の悪い書き方をしている．
- ```install_R.sh```は，```install_R_requirement.sh```の実行後に実行する．