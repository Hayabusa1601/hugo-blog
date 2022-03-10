+++
title = "ブログ開設（解説）あれこれ"
date = 2021-11-20T22:21:55+09:00
lastmod = 2022-03-06T21:43:00+09:00
author = "Hayabusa1601"
tags = ["hugo", "html"]
cover = ""
summary = "テーマのカスタマイズとか"
+++

## 目次 
<!-- omit in toc -->
- [目次](#目次)
- [このブログについて](#このブログについて)
- [テーマ](#テーマ)
  - [やらかし](#やらかし)
- [変更履歴](#変更履歴)
  - [フォントの変更](#フォントの変更)
- [参考文献](#参考文献)


## このブログについて

Hugo + Github pagesで作成しています。 また、Github Actionsで自動デプロイしています。  
Go が勉強できるかな～と軽い気持ちで始めましたが、待っていたのはクソ長HTMLと英語ドキュメンツとMarkdownでした。
  
内容に関しては、技術系のことやら趣味のことやらだらだら書いていこうと思います。  
技術者としては卵でほぼ素人なので、現時点ではこれからいろいろ学んで、ここをアウトプット場所にしていきたいな～と考えています。  
<br />  
<br />  
あとテスト勉強。

## テーマ

-  適用しているテーマ 
   -  &emsp;[MeiK2333/github-style](https://github.com/MeiK2333/github-style)  

サンプルページが見れなくなってる……？  
<br />  
<br />  

-  参考にしているテーマ 
   -  &emsp;[suihan74/github-style](https://github.com/suihan74/github-style)  

数少ない日本人のfork。
[カスタマイズ記録](https://suihan74.github.io/posts/2019/12_26_00_theme_customize/)とかもあって、英語弱者の私にはありがたいです。  
<br />  
<br />  

### やらかし  
**forkすればよかった**。  
私は比較的最近Githubを使用するようになったので、forkという神機能を知らず、そのままcloneして利用していました。
Github Actionsの設定をいじくっているときに気づきました、時すでに。  
<br />  
テーマのカスタマイズ等も、現状はわざわざthemeフォルダあるのにそのままブログのファイルに変更を加えているんですよね。  
今度せめてテーマを別でリポジトリ化とかするようにします。~~めんどくさいのでしばらくやらない~~
<br />  

でもforkしてもマージ作業が大変そう。
<br />  
<br />  

## 変更履歴
タグの表示とか、簡単にいろいろ変えたところはありますが、これから変更したところを記録していきます。
  
  ---
### フォントの変更
中華フォントだったので日本語フォントに変更しました。これ→[Koruri](https://koruri.github.io)  
<br />  
やり方は、font-familyが設定されているCSSを**上書きする**という力技です。  
もっといいやり方があるんやろうな（遠い目）
<br />  

1.  CSSがどこで読み込まれているか探す。  
    -  layouts/partials/head.htmlにありました。こんな感じのがたくさん  
    `<link rel="stylesheet" href='{{ "css/frameworks.min.css" | absURL }}' />`  
    config.tomlから読み込んでいるテーマもあるそうです、そっちのほうが便利そう。   
<br />   
<br />   

1.  head.htmlに以下のように追記する。  
    -  `<link rel="stylesheet" href='{{ "css/custom.css" | absURL }}' />`  
    これでcustom.cssが読み込まれるようになりました。  
<br />  
<br />  

1.  custom.cssをstatic/cssに作成  
      -  CSSをゴリゴリ書いていきます。  
    font-familyが指定されている場所を探して、それぞれフォントを上書きするコードを書きます。  
    マジで上書きしてるだけ→[static/css/custom.css](https://github.com/Hayabusa1601/hugo-blog/tree/main/static/css/custom.css) 

1.  フォントのファイルをwget


参考:  [WebfontをHUGOのブログに適用する](https://kasu-kasu.ga/post/enable-web-font/)

  



## 参考文献 
ここを作るにあたって参考にさせていただいた記事   
<br />  
[Hugoテーマのカスタマイズ箇所メモ &emsp;-すいはんぶろぐ.io](https://suihan74.github.io/posts/2019/12_26_00_theme_customize/)  

[WSL2 + Hugo + VSCodeでブログ更新環境を作った &emsp;-kapieciiのブログ](https://blog.kapiecii.com/posts/2020/04/20/wsl2-hugo-vscode/)  

[Hugo + GitHub Pages + GitHub Actions で独自ドメインのウェブサイトを構築する &emsp;-Zenn](https://zenn.dev/nikaera/articles/hugo-github-actions-for-github-pages)  

[ブログ移行日記(その3) - Hugoの設定と微調整(テーマに合わせた) &emsp;-@johtaniの日記 3rd](https://blog.johtani.info/blog/2020/01/24/setting-hugo/)  

<br />  
Hugoの公式ドキュメント(英語)  
[Hugo Documentation](https://gohugo.io/documentation/)  
