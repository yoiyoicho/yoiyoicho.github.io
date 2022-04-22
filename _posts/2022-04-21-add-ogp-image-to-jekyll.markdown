---
layout: post
title: "SNSシェアで必須のOGPイメージをJekyllサイトに設定してみた"
date: 2022-04-22 10:30:00 +0900
categories: Tech
tags: [Jekyll, Twitter]
image: /images/AdobeStock_189570981.jpeg
---

こんにちはYoshino（[@yoiyoicho](https://twitter.com/yoiyoicho)）です。つよいエンジニアになりたいという想いをこめて、アイコンをシャチのイラストに変更しました。モノクロだから、どんなデザインにもマッチしそうだしね。

さて、今回はブログをSNSにシェアする際に必須の「OGP（Open Graph Protocol）」をJekyllサイトに導入していきます。

![](/images/AdobeStock_189570981.jpeg)

## OGP（Open Graph Protocol）とは

そのページの概要を明示するHTML要素で、WebページをSNSでシェアした際に使われます。

例えばTwitterでは、このようなカード形式のリンクをよく見かけると思います。

![](/images/ScreenShot 2022-04-21 18.04.26.png)

これはTwitterカードと呼ばれるもので、Twitterが`<head></head>`内のogプロパティを持つmetaタグを参照して生成しています。

{% highlight html %}
<meta property="og:title" content="JekyllとGitHub Pagesで静的サイトを作成してみた" />
<meta property="og:locale" content="ja" />
<meta property="og:description" content="こんにちは。Webエンジニア転職を目指して学習＆個人開発中のYoshino（@yoiyoicho）です。" />
<meta property="og:url" content="http://yoiyoicho.github.io/tech/2022/04/18/hands-on-jekyll.html" />
<meta property="og:site_name" content="MY’s Tech Blog" />
<meta property="og:type" content="article" />
<meta property="og:image" content="https://yoiyoicho.github.io/assets/images/banners/home.jpeg">
<meta name="twitter:card" content="summary" />
<meta property="twitter:title" content="JekyllとGitHub Pagesで静的サイトを作成してみた" />
{% endhighlight %}

当サイトでは、初めからjekyll-seo-tagのプラグインを導入していたので、すでにog:titleやog:descriptionは自動生成されていました。

今回はさらに、og:imageの設定を詳しく見ていきます。

## Twitterカードの画像を大きくするには？

Twitterカードには大小があります。先ほどの例が小で、下が大です。

![](/images/ScreenShot 2022-04-22 10.56.37.png)

ビジュアル的にインパクトがあるので、大きい方を表示させたほうがよいでしょう。

[Twitter Developer Platformのドキュメント](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards)を読むと、`<head></head>`内で、`<meta name="twitter:card" content="summary_large_image" />`と指定すれば画像が大きく表示されることがわかりました。

さらに、jekyll-seo-tagプラグインのGitHubを確認すると、各ページでog:image要素を設定していれば、`<meta name="twitter:card" content="summary_large_image" />`を自動で吐き出すコードになっていることがわかりました。

## og:imageを記事毎に設定する

og:imageを設定するには、各記事のmarkdownファイルにimageの項を追加するだけ。

{% highlight markdown %}
---
layout: post
title: "SNSシェアで必須のOGPイメージをJekyllサイトに設定してみた"
date: 2022-04-22 10:30:00 +0900
categories: Tech
tags: [Jekyll, Twitter]
image: /images/AdobeStock_189570981.jpeg
---
{% endhighlight %}

こんな風に。

`bundle exec jekyll serve`コマンドを打ち、生成されたHTMLファイルを確認します。

{% highlight html %}
<!-- Begin Jekyll SEO tag v2.8.0 -->
<title>SNSシェアで必須のOGPイメージをJekyllサイトに設定してみた | MY’s Tech Blog</title>
<meta name="generator" content="Jekyll v3.9.0" />
<meta property="og:title" content="SNSシェアで必須のOGPイメージをJekyllサイトに設定してみた" />
<meta name="author" content="Minako Yoshino" />
<meta property="og:locale" content="ja" />
<meta name="description" content="こんにちはYoshino（@yoiyoicho）です。つよいエンジニアになりたいという想いをこめて、アイコンをシャチのイラストに変更しました。モノクロだから、どんなデザインにもマッチしそうだしね。" />
<meta property="og:description" content="こんにちはYoshino（@yoiyoicho）です。つよいエンジニアになりたいという想いをこめて、アイコンをシャチのイラストに変更しました。モノクロだから、どんなデザインにもマッチしそうだしね。" />
<link rel="canonical" href="http://localhost:4000/tech/2022/04/22/add-ogp-image-to-jekyll.html" />
<meta property="og:url" content="http://localhost:4000/tech/2022/04/22/add-ogp-image-to-jekyll.html" />
<meta property="og:site_name" content="MY’s Tech Blog" />
<meta property="og:image" content="http://localhost:4000/images/AdobeStock_189570981.jpeg" />
<meta property="og:type" content="article" />
<meta property="article:published_time" content="2022-04-22T10:30:00+09:00" />
<meta name="twitter:card" content="summary_large_image" />
<meta property="twitter:image" content="http://localhost:4000/images/AdobeStock_189570981.jpeg" />
<meta property="twitter:title" content="SNSシェアで必須のOGPイメージをJekyllサイトに設定してみた" />
<script type="application/ld+json">
{"@context":"https://schema.org","@type":"BlogPosting","author":{"@type":"Person","name":"Minako Yoshino"},"dateModified":"2022-04-22T10:30:00+09:00","datePublished":"2022-04-22T10:30:00+09:00","description":"こんにちはYoshino（@yoiyoicho）です。つよいエンジニアになりたいという想いをこめて、アイコンをシャチのイラストに変更しました。モノクロだから、どんなデザインにもマッチしそうだしね。","headline":"SNSシェアで必須のOGPイメージをJekyllサイトに設定してみた","image":"http://localhost:4000/images/AdobeStock_189570981.jpeg","mainEntityOfPage":{"@type":"WebPage","@id":"http://localhost:4000/tech/2022/04/22/add-ogp-image-to-jekyll.html"},"url":"http://localhost:4000/tech/2022/04/22/add-ogp-image-to-jekyll.html"}</script>
<!-- End Jekyll SEO tag -->
{% endhighlight %}

twitter:cardの項はsummary_large_imageになっており、og:image、twitter:imageの項も設定されています。

[Twitter Card Validator](https://cards-dev.twitter.com/validator)でプレビューを確認します。

![](/images/ScreenShot 2022-04-22 11.58.16.png)

いい感じですね。

## 感想

本当は、

- 記事に画像が1枚以上あるときは、一番頭の画像をogpイメージにする
- 記事に画像が1枚もないときは、デフォルトで用意しておいた画像をogpイメージにする

という設定にしたかったのですが、どうしてもyatとjekyll-seo-tagのあいだで衝突が起こってしまい、自力でhead.htmlを書き直すのもいやだったので、あきらめて各ページでogpイメージを設定することにしました。

うーん、なんか65点くらいの仕上がりですね。うまい解決方法があったら教えてほしいです。

なお当ブログにはコメント機能がありませんので、ご指摘があればTwitter（[@yoiyoicho](https://twitter.com/yoiyoicho)）のDM宛にいただけると幸いです。