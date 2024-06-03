---
title: GitHub Pagesの自動デプロイに失敗する(ブランチ名が"master")
author: naotoycd
pubDatetime: 2024-06-03T22:30:00Z
description: GitHub ActionsによるGitHub Pagesの自動デプロイで難儀したため解決方法をメモ
slug: githubpages-autodeploy-failed
tags:
  - GitHub
  - 備忘録
---

こんにちは。  
先日、Astroの[公式サイト](https://docs.astro.build/ja/guides/deploy/github/)を参考に、  
コミット後にGitHub ActionsでGitHub Pagesが自動デプロイするように作成したはずなのに、  
自動デプロイが実行されなかったため、コミットをする度に手動でデプロイをしていました。  
事象の原因と解決方法が分かりましたので下記に記載します。  

## 事象
GitHubのworkflowsフォルダにymlファイルを作成し、  
Settings >> Pages >> Build and deploymentのSourceの設定項目をGitHub Actionsに設定しているにも関わらず、コミット後に自動デプロイが実行されなかった。  

## 原因
GitHub Pagesのブランチの作成時期が古くGitHub Pagesのブランチ名が　**"master"** になっていたことが原因。[^1]  

現在、公式サイトなどに載っているworkflowsのymlファイルの記載例のブランチ名は通常 **"main"** となっており、上記ymlファイルの記載例をそのまま転用すると、実際のブランチ名は **"master"** だがymlファイル内の中身は **"main"** というブランチ名に差異がある状態になってしまい自動デプロイが実行されなかった。  

## 解決方法
GitHub Actionsを実行するGitHub Pagesのブランチ名とymlファイルの中身のブランチ名が合致していれば自動デプロイが実行されるようになりました。  

下記の２パターンの解決方法がありますのでどちらかの方法で修正してみてください。  
(私は②で修正をした後に①の方が根本的な解決法に思えたため①で修正し直しました)  

解決方法①：ブランチ名を"master"から"main"に変更する("main"に統一する方法)  

解決方法②：ymlファイル内のブランチ名を"main"から"master"に変更する("master"に統一する方法)  

以上です。

### 参考情報
GitHub、これから作成するリポジトリのデフォルトブランチ名が「main」に。「master」から「main」へ変更:https://www.publickey1.jp/blog/20/githubmainmastermain.html

ブランチの名前を変更する:
https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/managing-branches-in-your-repository/renaming-a-branch

### 備考
[^1]: デフォルトブランチ名について、2020年10月以降に作成されたGitリポジトリは **"main"** ,  
それ以前に作成されたGitリポジトリは **"master"** となっているようです。  
