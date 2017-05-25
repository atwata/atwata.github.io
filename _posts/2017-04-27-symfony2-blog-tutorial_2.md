---
layout: post
title: symfony2.8でブログチュートリアルを試す その2
categories: app
tags: php symfony
---

### ブログ閲覧ページの作成

その2はここから
[blogチュートリアル(5) ブログ閲覧ページの作成](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/05-list-page.html)

ここでやることは、ルーティングの作成とコントローラの作成。

ルーティングは

`/blog` でアクセスすると今回作成するブログのトップページを表示する。

ルーティングファイルの構成として、

メインのrouting.ymlから個別のルーティングファイルを読み込むようになっている

メインのファイル

```
$ cat app/config/routing.yml
my_blog:
    resource: "@MyBlogBundle/Resources/config/routing.yml"
    prefix:   /

app:
    resource: "@AppBundle/Controller/"
    type:     annotation
```

バンドル個別のファイル

```
$ cat src/My/BlogBundle/Resources/config/routing.yml 
my_blog_homepage:
    path:     /
    defaults: { _controller: MyBlogBundle:Default:index }
```

今回はメインのほうのprefixを変更

```
my_blog:
    resource: "@MyBlogBundle/Resources/config/routing.yml"
    prefix:   /
```

↓

```
my_blog:
    resource: "@MyBlogBundle/Resources/config/routing.yml"
    prefix:   /blog
```

この時点で

http://localhost:8008/app_dev.php/

ではなく

http://localhost:8008/app_dev.php/blog/

にアクセスできるようになっている。

次にバンドル側のルーティングも修正しておく

```
my_blog_homepage:
    path:     /
    defaults: { _controller: MyBlogBundle:Default:index }
```

↓


```
blog_index:
    path:     /
    defaults: { _controller: MyBlogBundle:Default:index }

blog_show:
    path:     /{id}/show
    defaults: { _controller: MyBlogBundle:Default:show }
```

blog_showという2つ目の設定ができてるが、対応するコントローラを作成していないのでまだ動かない。

自動作成されたコントローラを下記のように修正するらしい。

`src/My/BlogBundle/Controller/DefaultController.php`

```
<?php

namespace My\BlogBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;

class DefaultController extends Controller
{
    public function indexAction()
    {
        return $this->render('MyBlogBundle:Default:index.html.twig');
    }
}
```

↓

```
<?php

namespace My\BlogBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;

class DefaultController extends Controller
{
    public function indexAction()
    {
        $em = $this->getDoctrine()->getManager();
        $posts = $em->getRepository('MyBlogBundle:Post')->findAll();
        return $this->render('MyBlogBundle:Default:index.html.twig', array('posts' => $posts));
    }

    public function showAction($id)
    {
        $em = $this->getDoctrine()->getManager();
        $post = $em->find('MyBlogBundle:Post', $id);
        return $this->render('MyBlogBundle:Default:show.html.twig', array('post' => $post));
    }

}
```

説明もなく色んなことやってるなぁ。

いくつかポイント。

- symfony2では全てのDB操作はDoctrin2を通して行う。
- 実際の使い方としては
  - Doctorin2のEntityManagerオブジェクトを取得 `$this->getDoctrine()->getManager()`
  - EntityManager経由でRepositoryオブジェクトを取得して取得メソッドを呼ぶ `$em->getRepository('MyBlogBundle:Post')->findAll()`
- ビューを表示するには `$this->render()` 。
  - 第1引数にテンプレートのファイル名を指定
  - 第2引数にビュー側に渡すデータを配列で指定

ここまででDBから取得する処理が追加されたが、ビューに表示する処理を書いていないので、まだ画面には表示されない。

それにまだDBにデータが登録されていない。

次は、画面の表示処理。



### テンプレートの作成

[blogチュートリアル(6) テンプレートの作成](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/06-create-template.html)

symfony2のデフォルトのテンプレートはtwigである。

テンプレートファイル
`src/My/BlogBundle/Resources/views/Default/index.html.twig`
を修正する。

```
Hello World!
```

↓

{% raw %}
```
{# src/My/BlogBundle/Resources/views/Default/index.html.twig #}
<h1>Blog posts</h1>
<table>
    <tr>
        <td>Id</td>
        <td>Title</td>
        <td>CreatedAt</td>
    </tr>
    {# ここから、posts配列をループして、投稿記事の情報を表示 #}
    {% for post in posts %}
    <tr>
        <td>{{ post.id }}</td>
        <td><a href="{{ path('blog_show', {'id':post.id}) }}">{{ post.title }}</a></td>
        <td>{{ post.createdAt|date('Y/m/d H:i') }}</td>
    </tr>
    {% else %}
    <tr>
        <td colspan="3">No posts found</td>
    </tr>
    {% endfor %}
</table>
```
{% endraw %}


Twigではにforにelseをかけるのか。知らなかった。便利そう。

デフォルトで存在したindex用のテンプレートだけでなく2つ目のshow用のテンプレートも作成しておく

`src/My/BlogBundle/Resources/views/Default/show.html.twig`

{% raw %}
```
{# src/My/BlogBundle/Resources/views/Default/show.html.twig #}
<h1>{{ post.title }}</h1>
<p><small>Created: {{ post.createdAt|date('Y/m/d H:i') }}</small></p>
<p>{{ post.body|nl2br }}</p>
```
{% endraw %}

nl2brというtwigのフィルタを使用しているが、標準では読み込まれないらしく、
`app/config/config.yml`に下記を追加する必要あるらしい。

```
services:
    twig.extension.text:
        class: Twig_Extensions_Extension_Text
        tags:
            - { name: twig.extension }
```

設定してみたがエラー。

```
ClassNotFoundException in appDevDebugProjectContainer.php line 3057:
```

Twig_Extensions_Extension_Textが見つからないとのこと。
ひとまずスルーする。

次はDBのPOSTテーブルにデータを入れてみる。

```
insert into post values(1,'テスト投稿', '初めてのブログです！',sysdate,sysdate);
insert into post values(2,'今日はいい天気', '2回目のブログです！',sysdate,sysdate);
```

http://localhost:8008/app_dev.php/blog/

にアクセスすると記事が表示された。

http://localhost:8008/app_dev.php/blog/1/show

だと個別記事が表示される。


### 記事の追加

[blogチュートリアル(7) 記事の追加](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/07-create-form.html)

今までは記事の表示だけだったが、ここでは新規登録処理を行う。

表示だけより難易度は高そう。

まずはルーティング設定を追加。

`src/My/BlogBundle/Resources/config/routing.yml`

```
blog_new:
    pattern:  /new
    defaults: { _controller: MyBlogBundle:Default:new }
```

次にコントローラに新規登録用アクションを作成。

`src/My/BlogBundle/Controller/DefaultController.php`

```
use Symfony\Component\HttpFoundation\Request;
use My\BlogBundle\Entity\Post;

class DefaultController extends Controller
{

    public function newAction(Request $request)
    {
        $form = $this->createFormBuilder(new Post())
            ->add('title')
            ->add('body')
            ->getForm();

        if ('POST' == $request->getMethod()) {
            $form->submit($request);
            if ($form->isValid()) {
                // エンティティを永続化
                $post = $form->getData();
                $post->setCreatedAt(new \DateTime());
                $post->setUpdatedAt(new \DateTime());

                $em = $this->getDoctrine()->getManager();
                $em->persist($post);
                $em->flush();
                return $this->redirect($this->generateUrl('blog_index'));
            }
        }

        // 描画
        return $this->render('MyBlogBundle:Default:new.html.twig', array(
                'form' => $form->createView(),
        ));

    }
}
```

ポイントは、、、

- FormBuilderを使ってフォームオブジェクトを作成する
- フォームオブジェクトとはHTMLの入力フォームの概念をオブジェクト化している
- 今回のブログの例では `title` と `body` を要素として追加している
- フォームオブジェクトを使ってできること
  - バリデーション
  - DB(Doctorin)との連携
  - フォームウィジェットを利用したHTMLレンダリング
- `$form->submit($request)` することでリクエストパラメータがフォームオブジェクトにバインドされる（値がセットされる）
- `$form->isValid()` を行うことでバリデーションを行える（別途定義が必要）
- `$form->getData()` でPostエンティティオブジェクトを取得できる。
  - バインド済みなのでリクエストパラメータの値がセットされている

- DBに登録するには別途EntityManagerを生成し、Postエンティティを渡す。
  - `persist($post)` と `flush()` を行うことでDBに実際にinsertされる。
  
- ビューにはフォームオブジェクトをcreateViewしたものを渡している。
  - createView()するとFormView オブジェクトが生成される

ビューの作成。

`src/My/BlogBundle/Resources/views/Default/new.html.twig`

{% raw %}
```
<h1>Add Post</h1>
<form action="{{ path('blog_new') }}" method="post" {{ form_enctype(form) }} novalidate>
    {{ form_widget(form) }}
    <input type="submit" value="Save Post" />
</form>
```
{% endraw %}

- form_widgetにform(FormViewオブジェクト)を渡すと自動でテキストボックスやテキストエリアを生成してくれた。すごい。
  - 1つ1つテキストボックスやテキストエリアをViewに記述しなくてよいのか！



### データのバリデーション

[blogチュートリアル(8) データのバリデーション](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/08-form-validation.html)


Symfony2でのバリデーションの書き方2種類

- 設定ファイル(yml,xml)に書く
- モデル(Entity)にアノテーションとして書く


例ではアノテーションのパターンを試す

`src/My/BlogBundle/Entity/Post.php`


```
use Doctrine\ORM\Mapping as ORM;


...

    /**
     * @var string
     *
     * @ORM\Column(name="title", type="string", length=255)
     */
    private $title;

    /**
     * @var string
     *
     * @ORM\Column(name="body", type="text")
     */
    private $body;
...
```

↓

```
use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Validator\Constraints as Assert;

...

    /**
     * @var string
     *
     * @ORM\Column(name="title", type="string", length=255)
     * @Assert\NotBlank()
     * @Assert\Length(min=2,max=50)
     */
    private $title;

    /**
     * @var string
     *
     * @ORM\Column(name="body", type="text")
     * @Assert\NotBlank()
     * @Assert\Length(min=2)
     */
    private $body;
...

```


バリデーション用のコンポーネントがすでにあるので、そのクラスをアノテーションとして指定する

ちなみにNotBlankはこんな実装になっています。

```
cat vendor/symfony/symfony/src/Symfony/Component/Validator/Constraints/NotBlank.php 
<?php

/*
 * This file is part of the Symfony package.
 *
 * (c) Fabien Potencier <fabien@symfony.com>
 *
 * For the full copyright and license information, please view the LICENSE
 * file that was distributed with this source code.
 */

namespace Symfony\Component\Validator\Constraints;

use Symfony\Component\Validator\Constraint;

/**
 * @Annotation
 * @Target({"PROPERTY", "METHOD", "ANNOTATION"})
 *
 * @author Bernhard Schussek <bschussek@gmail.com>
 */
class NotBlank extends Constraint
{
    const IS_BLANK_ERROR = 'c1051bb4-d103-4f74-8988-acbcafc7fdc3';

    protected static $errorNames = array(
        self::IS_BLANK_ERROR => 'IS_BLANK_ERROR',
    );

    public $message = 'This value should not be blank.';
}
```

このバリデーションを指定した状態で未入力のままデータを登録しようとすると、

`This value should not be blank.`

とエラーになります。





### 記事の削除

[blogチュートリアル(9) 記事の削除](http://docs.symfony.gr.jp/symfony2/sf2-blog-tutorial/09-delete-post.html)

削除も基本的な構成としては追加と同じですね。

- ルーティングを追加し
- コントローラにアクションを追加し、
- 実際の削除はEntityManagerが行う
- EntityManagerにPostエンティティを渡し、`remove($post)` と `flush()` を行うことでDBからdeleteされる。

ソースの追加

`src/My/BlogBundle/Resources/config/routing.yml`

```
blog_delete:
    pattern:  /{id}/delete
    defaults: { _controller: MyBlogBundle:Default:delete }
```


`src/My/BlogBundle/Controller/DefaultController.php`

```
use Symfony\Component\HttpKernel\Exception\NotFoundHttpException;

class DefaultController extends Controller
{
    // ...
    public function deleteAction($id)
    {
        $em = $this->getDoctrine()->getManager();
        $post = $em->find('MyBlogBundle:Post', $id);
        if (!$post) {
            throw new NotFoundHttpException('The post does not exist.');
        }
        $em->remove($post);
        $em->flush();
        return $this->redirect($this->generateUrl('blog_index'));
    }
}
```

`src/My/BlogBundle/Resources/views/Default/index.html.twig`

{% raw %}
```
<h1>Blog posts</h1>
<table>
    <tr>
        <td>Id</td>
        <td>Title</td>
        <td>CreatedAt</td>
        <td>Operation</td>
    </tr>
    {# ここから、posts配列をループして、投稿記事の情報を表示 #}
    {% for post in posts %}
    <tr>
        <td>{{ post.id }}</td>
        <td><a href="{{ path('blog_show', {'id':post.id}) }}">{{ post.title }}</a></td>
        <td>{{ post.createdAt|date('Y/m/d H:i') }}</td>
        <td><a href="{{ path('blog_delete', {'id':post.id}) }}">Delete</a></td>
    </tr>
    {% else %}
    <tr>
        <td colspan="4">No posts found</td>
    </tr>
    {% endfor %}
</table>

<div>
<a href="{{ path('blog_new') }}">add post</a>
</div>
```
{% endraw %}



