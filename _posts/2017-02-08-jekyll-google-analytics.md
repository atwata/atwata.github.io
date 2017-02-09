---
layout: post
title: jekyllで作成したサイトにGoogle Analyticsを設定する方法
categories: blog
tags: jekyll
---

Google AnalyticsをJekyllで作成したサイトに設定したい。

やるべきことは3つ。


## 1.Google Analyticsのサイトで新しいプロパティを作成し、トラッキングIDを発行。

これはJekyllに限ったことではないので詳細は割愛。

詳細はこの辺りの公式ページを見ていただくと良いと思う。
<https://support.google.com/analytics/answer/1042508?hl=ja>


## 2.トラッキングするためのスクリプトを読み込むためのincludeファイル作成

ファイルは_includesディレクトリに配置する。今回はanalytics.htmlという名前とする。

```
├── _includes
│   ├── analytics.html
```

analytics.htmlには上記の1で作成した際に画面に表示されるトラッキングするためのjavascriptをコピペする。

```
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-XXXXXXXX-X', 'auto');
  ga('send', 'pageview');

</script>
```

UA-XXXXXXXX-Xについては自分のトラッキングIDにしてください。

## 3.全てのページのベースであるdefalt.htmlで上記で作成したincludeファイルを読み込む

default.htmlは_laytoutフォルダにある。

bodyタグの直後に次のように記述し、Google Analyticsのスクリプトを読み込むようにする。

{% raw %}
```
<body>
    {% include analytics.html %}
 ...
  </body>
</html>
```
{% endraw %}

上記でも問題はないのだが、localhostでサイトをjekyll serveで表示した際も読み込まれてしまう。
本番環境でのみ読み込ませたい場合は下記のように条件文を追加すると良い。

```
{% raw %}
<body>
    {% if jekyll.environment == 'production' %}
    {% include analytics.html %}
    {% endif %}
...
 </body>
</html>
{% endraw %}
```


以上で各サイトで本番環境のみGoogleAnalyticsのスクリプトが読み込まれる。
