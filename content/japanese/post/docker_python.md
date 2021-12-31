---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Python3の開発環境をDocker containerとして作る"
subtitle: ""
summary: ""
authors: []
tags: [Python, Docker]
categories: [Docker]
date: 2021-07-08T18:58:51+09:00
lastmod: 2021-07-08T18:58:51+09:00
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
この記事では，Docker containerとしてPythonの開発環境を作ったときのメモを残す．
この記事の内容は，GitHubのサブのアカウントの[publicリポジトリ](https://github.com/tcbn-ai/Docker_python_env)で公開している．

### 1. 必要なもの
- Docker Engineのインストール
- VSCodeのインストール
  - Remote Developementの導入

### 2. 基本構成
- Docker Image
    - [python](https://hub.docker.com/_/python)の[3.8.10-buster](https://github.com/docker-library/python/blob/e0e01b8482ea14352c710134329cdd93ece88317/3.8/buster/Dockerfile)を使っている．
- ディレクトリ構成
    ```
    |- test_code/   # コードを置いている
    |- .devcontainer/
        |- devcontainer.json
        |- docker-compose.yml
        |- dockerfile
        |- requirements.txt
    ```

#### 各種ファイル
- ```devcontainer.json```
    - VSCodeのRemoteを使うときの設定
      ```json
      // For format details, see https://aka.ms/devcontainer.json. For config options, see the README at:
      // https://github.com/microsoft/vscode-dev-containers/tree/v0.177.0/containers/docker-existing-dockerfile
      {
        // 名前は任意
        "name": "Docker-Python",
        // dockercomposefileの場所 (同階層に置いている)
        "dockerComposeFile": "docker-compose.yml",
        // 使う拡張機能
        "extensions": [
          "ms-python.python"
        ],
        // ここに記載している"service"名とdocker-compose.ymlに記載している"service"を一致させる
        "service": "python",
        // コンテナ内に入ったときのworkdir
        "workspaceFolder": "/code",
        // VSCodeを閉じたときのアクション
        "shutdownAction": "stopCompose"
      }
      ```
- ```docker-compose.yml```
    - 複数のコンテナを定義し，実行することができる．
    - 今回は，1つのコンテナに対する処理を記述
      ```yml
      version: "3"    # 3が最新版
      services: 
          python: # ここの名前とdevcontainer.jsonの"service"を一致させる
              build: .    # 同階層のdockerfileからビルドする
              command: sleep infinity
              volumes: 
                  - ../:/code  # 上階層のディレクトリをDocker Container上のworkdirにマウント
              environment: 
                  SHELL: /bin/bash
      ```
- ```dockerfile```
    - コンテナを作るための処理
      ```dockerfile
      ########### Image file ###########
      FROM python:3.8.10-buster
      ##################################

      ########### update and install packages ###########
      # apt-get upgradeに-yを付けないとexit 1になります．追加しました．(5/23)
      RUN apt-get update && \
          apt-get -y upgrade && \
          apt-get install -y vim git && \
          rm -rf /var/lib/apt/lists*
      ###################################################

      ########### create workspace ###########
      RUN mkdir /code
      WORKDIR /code
      ADD ./requirements.txt /code/
      ########################################

      ########### install packages via pip ###########
      RUN pip3 install -r requirements.txt
      ADD . /code/
      ################################################
      ```
- ```requirements.txt```
    - ```pip install```するファイル
      ```txt
      ############ Requirements Packages ############
      pylint
      numpy
      scipy
      sympy
      matplotlib
      statsmodels
      sklearn
      pandas
      networkx
      ```


### 3. 使用時
- ```test_code```と同階層に自分のコードを格納したフォルダを配置して，VSCodeの左下の"Open Remote Window"を選択．
- Reopen in Containerを選択．
- ワークスペースが開いたら，terminalを開き，```python *.py```で実行する．

### 4. いろいろ変えたいとき
- ```pip install```するパッケージを変更したいとき
    - ```requirements.txt```を書き換える
- Pythonのバージョンを3の別バージョンに変えたいとき
    - ```dockerfile```のイメージファイル (2行目の```FROM```以下) を書き換える．
- Pythonのバージョンを2にしたいとき
    - [python](https://hub.docker.com/_/python)には2系がないので，イメージファイルをUbuntu等にして1からインストールする．