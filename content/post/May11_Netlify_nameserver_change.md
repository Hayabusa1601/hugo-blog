+++
title = "NetlifyのネームサーバーをNetlify DNSからお名前.comに変更する方法"
date = 2022-05-11T00:56:20+09:00
lastmod = true
author = "Hayabusa1601"
tags = ["DNS"]
cover = ""
summary = "Netlify DNSからお名前.comにネームサーバーを変更"
+++


# NetlifyのネームサーバーをNetlify DNSからお名前.comに移動

## 目次

- [NetlifyのネームサーバーをNetlify DNSからお名前.comに移動](#netlifyのネームサーバーをnetlify-dnsからお名前comに移動)
  - [目次](#目次)
  - [なぜやろうとしたか](#なぜやろうとしたか)
  - [前提事項](#前提事項)
  - [手順](#手順)
  - [補足](#補足)


## なぜやろうとしたか
- このブログに設定するサブドメイン設定したかったから。
- Netlify DNSで設定しているとサブドメインが外部サービスで設定できないらしい

## 前提事項
- ここで操作しているドメインは、お名前.comで取得したドメインである。

## 手順
1. Netlify DNSを解除する。
	1. NetlifyのDNS設定からOptions→Set up Netlify DNSを選択
	2. 一番下にある、Delete DNS zoneを選択
2. DNSレコードを設定する
	1. まずはwwwサブドメインから(CNAME)
		1. wwwがついているサブドメインの check DNS configurationを押すと、DNSレコード設定が表示される。
		2. お名前.comのNaviから、DNSレコード設定画面に移動し、先程のDNSレコード設定で表示されたCNAMEレコードを登録する。
		3. [参考](https://laboratory.kazuuu.net/how-to-set-up-your-own-domain-on-netlify-with-your-name-com/)
	2. 次にAレコードで設定
		1. wwwサブドメインの設定で行ったのと同じように、wwwがついていない方のドメインのcheck DNS configurationをクリックして、DNSレコード設定を表示する。
		2. お名前.comのNavi から、DNSレコード設定画面に移動して、先程のDNSレコード設定で表示された、ピリオドで４つに区切られたAレコードを入力する。ホスト名は空欄で良い。
		3. [参考](https://contentful-explore.netlify.app/how/netlify-domain-setting)
3. ロード画面が入ったら完了


## 補足
なんか怒られた
{{< figure src="/images/post/Mar11/dns_error.png" width="800" >}}
どうやら昔使ってたドメインの設定が残っててうまくhttps化できない？

Teamのトップページ→Domainsから昔使っていたドメインのDNS zoneを削除することで解決した。
[参考？非アクティブなDNS zoneがある場合の対処法（英語）](https://answers.netlify.com/t/support-guide-how-to-detect-and-fix-inactive-netlify-dns-zones/21742)
