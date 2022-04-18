---
layout: post
title:  "JekyllとGitHub Pagesで静的サイトを作成してみた"
date:   2022-04-18 16:55:56 +0900
categories: jekyll ruby
---
こんにちは。Webエンジニア転職を目指して学習＆個人開発中のYoshino（[@yoiyoicho](https://twitter.com/yoiyoicho)）です。

今回は、プログラミングスクールRUNTEQの勉強会で教えてもらったJekyll＋GitHub Pagesを使い、個人用テックブログとして使う静的なWebサイト（このサイト）を作成したので、その記録をここに残そうと思います。

## Jekyllとは

静的なWebサイトをコマンド一つで作成することができるRubyのgem。

Markdownなどのマークアップ言語で書かれたテキストを用意すれば、Jekyllがレイアウトを合成してHTMLファイル一式を作成してくれます。

WordPressなどのCMSに比べて、自動化しすぎず、ほどよく手元でコントロールできるのがポイントのよう。

日本語ガイドが用意されているので、そちらを見るとイメージがつかめます。

- [http://jekyllrb-ja.github.io/](http://jekyllrb-ja.github.io/)
- [https://www.codegrid.net/articles/jekyll-1/](https://www.codegrid.net/articles/jekyll-1/)

## GitHub Pagesとは

GitHubが提供する、静的サイトのホスティングサービス。ローカルからGitHubにソースコードをpushすれば、即座にWebサイトが公開されます。

## Jekyllのインストール

それではさっそくJekyllを使った静的Webサイトを作成していきましょう。

ローカルの作業ディレクトリで下記のコマンドを打ちます。

{% highlight shell %}
# jekyllをインストール
gem install bundler jekyll
{% endhighlight %}

インストールには数分かかります。

## GitHubとローカルにリポジトリを作成

次に、GitHub側で今回のWebサイトで使用するリポジトリを作成します。Github Docsに詳しい手順が載っているのでその通りに進めていきます。

- [https://docs.github.com/ja/github-ae@latest/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll](https://docs.github.com/ja/github-ae@latest/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)

新しいリポジトリを作成し、Repository nameは`[GitHubのユーザー名].github.io`に、公開範囲は`Public`を選択します。

続いて、ローカルの作業ディレクトリに移動し、下記のコマンドを打ってローカルリポジトリを作成します。

{% highlight shell %}
git init [GitHubのユーザー名].github.io
{% endhighlight %}

## JekyllサイトをGitHub Pages用にカスタマイズして作成

どんどん行きましょう。作業ディレクトリで下記のコマンドを打ち、新しいJekyllサイトを作成します。

{% highlight shell %}
# jekyllのプロジェクトを作成
jekyll new --skip-bundle .
{% endhighlight %}

ここで、作成されたGemfileをGitHub Pages用に編集して保存します。

{% highlight ruby %}
# 以下の行をコメントアウト
#　gem "jekyll", "~> 4.2.2"

# 以下の行をコメントアウトから外す
gem "github-pages", group: :jekyll_plugins
{% endhighlight %}

コンソールで`bundle install`を実行すれば、GitHub Pagesに対応したJekyllサイトの作成が完了します。

## Jekyllサイトをローカルでチェックする

ここまで来たらもう少し！　コンソールで次のコマンドを打って、ローカルにサーバーを立てましょう。

{% highlight shell %}
# ローカルにサーバーを立てる
bundle exec jekyll serve
{% endhighlight %}

ブラウザを開き、http://localhost:4000にアクセスすれば、初期設定のままのJekyllサイトが立ち上がっているはずです。

以後、markdownファイルや_config.ymlなどに変更を加えた場合、`Ctrl+C`でサーバーを一度終了し、再び`bundle exec jekyll serve`でサーバーを立てないと、ブラウザからJekyllサイトの変更を確認することができません。

`bundle exec jekyll serve`のコマンドを打つたびに、HTMLファイルが作り直されるイメージです。

## themeの変更

さて、ここから自分流にJekyllをカスタマイズしていきましょう。

Jekyllはローンチされてから年月が経っているフレームワークということもあり、無料のテーマが充実しています。下記のサイトによくまとまっていました。

- [http://jekyllthemes.org/](http://jekyllthemes.org/)
- [https://jamstackthemes.dev/ssg/jekyll/](https://jamstackthemes.dev/ssg/jekyll/)

私は今回、yet another theme（yat）というテーマを選びました。

- [https://github.com/jeffreytse/jekyll-theme-yat](https://github.com/jeffreytse/jekyll-theme-yat)

READ.meの「Remote Theme Method with GitHub Pages」の項目の通りに進めます。

まず、Gemfileを編集します。

{% highlight ruby %}
# 初期設定のテーマ、minimaをコメントアウト
# gem "minima", "~> 2.5"

# 以下の1行を追記
gem "github-pages", group: :jekyll_plugins
{% endhighlight %}

続いて、_config.ymlを編集。

{% highlight yml %}
# ここでもminimaをコメントアウト
# theme: minima

# 以下の1行を追記
remote_theme: "jeffreytse/jekyll-theme-yat"
{% endhighlight %}

コンソールで`bundle install`を実行。

最初は、READ.meを流し読みして「Gem-based Theme Method」の方法で実装してしまっていたため、後述するGitHubへpushした際にエラーが出て詰まってしまいました。新しいものを使うとき、READ.meはよく読みましょう（反省点）。

## _config.ymlをカスタマイズ

metaデータやヘッダー画像など、サイト全体に共通する部分は、_config.ymlで設定します。私は下記のように設定しました。

特に注意しなければならないのは、`defaults>home>banner`の項で、デフォルトでは値が設定されていないのか、ここにヘッダー画像のパスを設定しないと、サイトがテンプレート通りに表示されませんでした。

{% highlight yml %}
title: MY's Tech Blog
author: Minako Yoshino

copyright: "Copyright © 2022 Minako Yoshino"

description: >- # this means to ignore newlines until "baseurl:"
  Webエンジニア転職と個人開発の記録を発信します。

baseurl: "" # the subpath of your site, e.g. /blog
url: "https://yoiyoicho.github.io" # the base hostname & protocol for your site, e.g. http://example.com
domain: yoiyoicho.github.io

# If you want to generate website sitemap, you can set true
sitemap: true

# If you want to change site language, you can set lang option
lang: "ja"  # default lang is en

# Page default value
defaults:
  home:
    heading: "MY's Tech Blog"
    subheading: "Webエンジニア転職と個人開発の記録"
    banner: "/assets/images/banners/home.jpeg"

# Build settings
# highlighter: none
markdown: kramdown
kmarkdown:
  input: GFM

plugins:
  - jekyll-feed
  - jekyll-seo-tag
  - jekyll-sitemap
  - jekyll-paginate
  - jekyll-spaceship
{% endhighlight %}

このように_config.ymlに設定を書いておくと、`bundle exec jekyll serve`したときに、_config.ymlを読み込んでJekyllがHTMLを書き出してくれます。

## GitHub Pagesで公開

ローカルでのチェックが終わったら、GitHubにpushして公開しましょう！

Jekyllプロジェクトと同じディレクトリで下記のコマンドを打てばOKです。

{% highlight shell %}
git add .
git commit -m '[お好きなメッセージ]'
git remote add origin　https://github.com/[GitHubのユーザー名]/[GitHubのユーザー名].github.io.git
git push -u origin master
{% endhighlight %}

## 感想

見栄えのよいオリジナルのWebサイトが簡単に作れました。

Google Analyticsを入れてアクセス解析もできるし、広告が出ないのもよいです。

レスポンスが早いのもGood。

ただブログとして考えた場合、Markdown形式で書くのは少し慣れが必要そうです。

また、外部の記事を引用した際に、他のCMSがデフォルトで提供しているような、OGP画像を取得してカード形式で表示、ということもできません（自分でメソッドを書けばできそう）。

Markdownを使った記事の更新方法はまた別の記事で書いていこうと思います。

なお当ブログにはコメント機能がありませんので、ご指摘があればTwitter（[@yoiyoicho](https://twitter.com/yoiyoicho)）のDM宛にいただけると幸いです。