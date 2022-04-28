+++
title = "GitHub Pagesで作ったサイトに独自ドメイン設定した"
date = 2021-12-12T12:40:29+09:00
lastmod = 2022-04-28T18:31:00+09:00
author = "Hayabusa1601"
tags = ["hugo", "DNS"]
cover = ""
summary = " "
+++

## 目次
- [目次](#目次)
- [はじめに](#はじめに)
- [ドメインのDNS設定を変更する](#ドメインのdns設定を変更する)
- [独自ドメインをGitHub Pagesに設定](#独自ドメインをgithub-pagesに設定)
- [デプロイのフローを変更](#デプロイのフローを変更)
- [追記](#追記)
- [参考文献](#参考文献)
    - [GitHub公式](#github公式)
    - [記事](#記事)

## はじめに
[hayablog.xyz](hayablgo.xyz)  
  
最初は変更履歴に書いてたんですが、思いの外長くなったので別記事にしました。
  
[お名前.com](onamae.com)で取得しました。設定を変更するたびカードの有効性の確認がきてちょっと怖かったです。  
  
最初はCNAMEレコードだけ設定して、「転送Plus」機能でURLを転送しようとしましたが、どうやら110円かかるそうなのでやめました（金欠）。  
  
たぶんというか絶対どうやったか忘れるのでGitHub pagesで独自ドメインを取得する方法の備忘録です。  

## ドメインのDNS設定を変更する
~~なんかいい感じに~~ドメインを管理しているネームサーバーからDNSレコードの設定をします。
  
お名前.comでは、
設定したいドメインの設定から「ネームサーバーの設定」の「DNSレコード設定を利用する」から設定できます。

{{< figure src="/images/post/Nov20_theme_customized/onamae_dnsrecords.png" class="center" >}}
  
+ wwwサブドメインをつけたいのでCNAMEレコードを設定します。 


| ホスト | TYPE | VALUE |
|:-----------|------------:|:------------:|
| www.hayablog.xyz  | CNAME        | hayabusa1601.github.io         |  
  
  こんなかんじ

+ Apexドメインを使用するためにAレコードを追加します。  
  
| ホスト | TYPE | VALUE |
|:-----------|------------:|:------------:|
| hayablog.xyz    | A      | 185.199.108.153      |
| hayablog.xyz    | A      | 185.199.109.153      |
| hayablog.xyz    | A      | 185.199.110.153      |
| hayablog.xyz    | A      | 185.199.111.153      |
  
AレコードのIPアドレスについては[GitHub公式Docs](https://docs.github.com/ja/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain)からそのまま持ってきます。 

  確認画面  
  {{< figure src="/images/post/Nov20_theme_customized/DNS_setting.png" >}}
  

digコマンド使って確認した画面
{{< figure src="/images/post/Nov20_theme_customized/dig_dnscheck.png" >}}
## 独自ドメインをGitHub Pagesに設定

.github.ioでおわるリポジトリのSettings→Pagesに移動します。  
  
「Custom domain」に取得したドメインを入力し、保存します。  
「Enforce HTTPS」でHTTPS化できます。よく知らないけど有効になるまでちょっと時間がかかるらしいです。
  
{{< figure src="/images/post/Nov20_theme_customized/pages_customdomain.png" >}}

## デプロイのフローを変更
私はGitHub Actionsで、GitHub Pagesにデプロイしているので、デプロイ時の操作フローを独自ドメインに対応させます。  

  
デプロイ元にあるフローのファイルに以下のように書き足します。

{{< figure src="/images/post/Nov20_theme_customized/flow_cname.png" >}}

これでデプロイ時に独自ドメインを参照するようになりました。

## 追記

もともと持っていたhayabusa1601.netのサブドメインに再設定しました。

## 参考文献
#### GitHub公式

[カスタムドメインとGitHub Pages のトラブルシューティング](https://docs.github.com/ja/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages)  
[GitHub Pages サイトのカスタムドメインを管理する](https://docs.github.com/ja/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)  
[GitHub Pages for GitHub Action](https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-add-cname-file-cname)  


#### 記事

[GitHub Pagesで静的サイトを作る](https://blog.negativemind.com/2020/03/28/create-static-website-with-github-pages/)  
[GitHub Pagesに独自ドメインを設定してHTTPS化する](https://mae.chab.in/archives/60095)  
[GitHub Pagesを独自ドメイン化 + Https化](https://takumon.com/2018/09/12/)  
[GitHub Pagesに独自ドメインを設定する方法](https://zenn.dev/donchan922/articles/59c54fe659128294bb65)  
