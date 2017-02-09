---
layout: post
title: jekyllで記事を投稿する方法
categories: tool
tags: jekyll
---


ファイル名が大事。

```
YEAR-MONTH-DAY-title.MARKUP
```

という形式にする必要がある。

以下のようなファイル名と中身でファイルを作成する。

````
```
$ cat _posts/2017-02-07-hello.md
---
layout: default
title: Hello world
---

## hello subject

- aaa
- bbb
  - ccc

```
print hello world
```
````

githubにpushする。

作成されたページのキャプチャはこちら。

![20170207_github_repos]({{site.baseurl}}/images/20170207_site_index.png)

![20170207_github_repos]({{site.baseurl}}/images/20170207_site_page.png)
