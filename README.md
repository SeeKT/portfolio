# ウェブサイト
Hugo を使った Website の構築

準備
```
hugo new site [repository name]
```
```
git init
```
```
git submodule add https://github.com/lxndrblz/anatole.git themes/anatole
```

ローカルで確認
```
hugo server
```

本番環境
```
hugo
```


新しいコンテンツの作成
```
hugo new content/post/[contant name].md
```
または
```
hugo new post/[contant name]
```