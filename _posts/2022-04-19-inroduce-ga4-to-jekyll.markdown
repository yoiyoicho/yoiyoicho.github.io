---
layout: post
title:  "Google Analytics 4（GA4）をJekyllサイトに導入してみた"
date:   2022-04-19 10:32:00 +0900
categories: Jekyll GA4
---
念願の個人ブログを開設することができ、テンションブチ上がり中のYoshino（[@yoiyoicho](https://twitter.com/yoiyoicho)）です。

やはり記事を書いたらどのくらい読まれたのか気になるということで、今回はアクセス解析を入れてみたいと思います。

## Google Analytics 4（GA4）とは

Googleが提供する次世代測定ソリューション。前身のユニバーサルアナリティクス（旧GA、UA）のサポートが2023年7月に終了すると突如発表され、Webマーケティング業界で激震が走ったとかどうとか。

GA4はUAの後継となりますが、操作感やレポートの見方は一新されているそうです。私は前職のWeb編集時代にUAを使っていたので、UAの扱いにはかなり慣れていたものの、GA4は全く違うものとなれば早めにキャッチアップしておきたいところ。ということで、当ブログにGA4を導入することにしました。

- [ユニバーサル アナリティクスのサポートは終了します](https://support.google.com/analytics/answer/11583528)
- [[GA4] Google アナリティクスの基本操作](https://support.google.com/analytics/answer/9367631)

## GA4で新しいプロパティを作成し、トラッキングIDを発行する

まずはGA4の設定から進めていきます。Googleの公式ドキュメントに書いてある手順にしたがって、アナリティクスのアカウントを作成して、GA4のプロパティを作成します。

- [[GA4] アナリティクスで新しいウェブサイトまたはアプリのセットアップを行う](https://support.google.com/analytics/answer/9304153)

## _config.ymlに測定IDを埋め込むがうまくいかない

私が使っているyet another theme（yat）というテーマのGitHubを確認すると、テーマ側でGAのスクリプトを用意してくれている形跡があったので、まずは_config.ymlにG-から始まる測定IDを記入してみます。

{% highlight yml %}
# Google analytics
google_analytics: [G-から始まる測定ID]
{% endhighlight %}

そして、ローカルでサーバーを立ち上げてページビューを発生させました。が、GA4側はうんともすんとも。まるで無風です。yatのリポジトリにあるGA関係のソースコードがまだUA仕様になっているようで、うまく機能しません。

![](/images/スクリーンショット 2022-04-19 10.18.42.png)

## _config.ymlを上書きする

再びGA4公式セットアップガイドに帰り、`データ収集を設定する（ウェブサイトの場合）＞グローバル サイトタグをウェブページに直接追加する`の項目を確認します。

というコードをヘッダーに埋め込めばよいみたい。

yatのGitHubを確認し、Google。この部分を上書きします。

## 感想

ああああ。

なお当ブログにはコメント機能がありませんので、ご指摘があればTwitter（[@yoiyoicho](https://twitter.com/yoiyoicho)）のDM宛にいただけると幸いです。