---
layout: post
title:  "Google Analytics 4（GA4）をJekyllサイトに導入してみた"
date:   2022-04-19 11:17:00 +0900
categories: Jekyll GA4
---
念願の個人ブログを開設することができ、テンションブチ上がり中のYoshino（[@yoiyoicho](https://twitter.com/yoiyoicho)）です。

やはり記事を書いたらどのくらい読まれたのか気になるということで、今回はアクセス解析を入れてみます。

## Google Analytics 4（GA4）とは

Googleが提供する次世代測定ソリューション。前身のユニバーサルアナリティクス（旧GA、UA）のサポートが2023年7月に終了すると突如発表され、Webマーケティング業界で激震が走ったとかどうとか。

GA4はUAの後継となりますが、操作感やレポートの見方は一新されているそうです。私は前職のWeb編集時代にUAを使っていたので、UAの扱いにはかなり慣れていたものの、全く違うとなれば早めにキャッチアップしておきたいところ。ということで、当ブログにGA4を導入することにしました。

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

## _includes/extensions/google-analytics.htmlを上書きする

再びGA4公式セットアップガイドに帰り、`データ収集を設定する（ウェブサイトの場合）＞グローバル サイトタグをウェブページに直接追加する`の項目を確認します。

{% highlight html %}
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=[測定ID]"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', '[測定ID]');
</script>
{% endhighlight %}

上のコードを測定したいページの`<head></head>`内に埋め込めばよいみたい。

yatのGitHubを確認し、GAに関するソースコードは`_includes/extensions/google-analytics.html`に記載されていることがわかりました。この部分をGA4仕様に上書きします。

{% highlight html %}
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id={% raw  %}{{ site.google_analytics }}{% endraw %}"></script>
<script>
  function initGoogleAnalytics() {
    var doNotTrack = (window.doNotTrack === "1" || navigator.doNotTrack === "1" ||
      navigator.doNotTrack === "yes" || navigator.msDoNotTrack === "1");
    var enableDNT = "{{ site.enableDNT | default: true }}" == "true";

    if (!enableDNT || !doNotTrack) {
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', '{% raw  %}{{ site.google_analytics }}{% endraw %}');
    }
  }
  window.addEventListener("load", initGoogleAnalytics);
</script>
{% endhighlight %}

こうすれば、_config.ymlで記載した測定IDを、_includes/extensions/google-analytics.htmlの{% raw  %}{{ site.google_analytics }}{% endraw %}の部分で読み込んで、全てのページに展開してくれます。

設定が終わったら、GitHubにpush。

![](/images/スクリーンショット 2022-04-19 11.06.27.png)

GA4でアクセスが確かめられました。スマホでサイトを開いていたので、今いる場所がばばーんと表示されていてびっくり。

なんでもトラッキングされて、こわい時代になりましたね〜。

## 感想

開発環境の数値が混じっていないかとか気になるところはありますが、とりあえずサイトにGA4を導入をすることができました。

噂に聞いていた通り、GA4の管理画面はUAと全く違いました。

GA4はエンジニアでも知っていて損はない知識ですので、2023年夏の完全切り替えまでにキャッチアップしていきたいなと思っています。

またこのブログに関しては、次はOGP画像の設定をする予定です。どんどん改造していくぞ〜！

なお当ブログにはコメント機能がありませんので、ご指摘があればTwitter（[@yoiyoicho](https://twitter.com/yoiyoicho)）のDM宛にいただけると幸いです。