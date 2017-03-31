---
layout: post
title: jekyllでサイト内検索できるようにする（Google Custom Search）
categories: blog
tags: jekyll google
---

## GoogleCustomSearch作成

Google カスタム検索エンジン

<https://cse.google.co.jp/cse/?hl=ja>

上記サイトで手順に従いカスタム検索エンジンを作成する。

詳細は割愛しますがデザイン等もカスタマイズできます。

コードを生成してくれるので、それを自分のサイトに貼り付けます。

## サイトに適用

デフォルトで存在したabount.mdを参考にsearch.mdを作成。

内容は下記の通り。


```
---
layout: page
title: Search
permalink: /search/
---

<script>
  (function() {
    var cx = '007943044707105383416:msvguhsgrc4';
    var gcse = document.createElement('script');
    gcse.type = 'text/javascript';
    gcse.async = true;
    gcse.src = 'https://cse.google.com/cse.js?cx=' + cx;
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(gcse, s);
  })();
</script>
<gcse:search></gcse:search>


```

※実際のコードはGoogle Custom Searchで作成したコードを貼り付けてください。

するとデフォルトでサイドバーにも表示され、下記の通り検索可能となった。

![20170331_google_custom_search]({{site.baseurl}}/images/20170331_google_custom_search.png)