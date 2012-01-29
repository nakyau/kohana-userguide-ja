# Hello, World

ほとんど全てのフレームワークは"Hello,World"の例を含んだドキュメントを含んでいます。
したがって、私たちがこの伝統を破ることはかなり無作法でしょう!

非常に基礎的なHello Worldを作成することから始めて、そして次にそれをMVCの原理に従うように拡張します。

<div class="original-doc">
Just about every framework ever written has some kind of hello world example included, so it'd be pretty rude of us to break this tradition!

We'll start out by creating a very very basic hello world, and then we'll expand it to follow MVC principles.
</div>

## 要点 {#bare-bones}
最初にKohanaがリクエストを処理できるようにコントローラーを作成しなくてはなりません。
applicationフォルダに以下のように記述した`application/classes/controller/hello.php`ファイルを作成します。
<div class="original-doc">
## Bare bones

First off we have to make a controller that Kohana can use to handle a request.

Create the file `application/classes/controller/hello.php` in your application folder and fill it out like so:
</div>

	<？php defined('SYSPATH') OR die('No Direct Script Access');

	Class Controller_Hello extends Controller
	{
		public function action_index()
		{
			echo 'hello, world!';
		}
	}


ここで何が行われているか見てみましょう。

<div class="original-doc">
Lets see what's going on here:
</div>

`<?php defined('SYSPATH') OR die('No Direct Script Access');`

: 最初のphpを開始するタグはお分かりでしょう。（分からない場合はおそらく[phpを学習する](http://php.net)必要があります。）phpの開始タグに続くものはこのファイルがKohanaによってインクルードされているかを簡単にチェックするものです。これはURLから直接このファイルにアクセスすることを防ぎます。
<div class="original-doc">
:	You should recognize the first tag as an opening php tag (if you don't you should probably [learn php](http://php.net)).  What follows is a small check that makes sure that this file is being included by Kohana.  It stops people from accessing files directly from the url.
</div>


`Class Controller_Hello extends Controller`
: この行はコントローラーを宣言しています。全てのコントローラークラスは`Controller_`というプリフィックスが付き、コントローラーがあるフォルダへのパスをアンダースコアで区切って表したものになります。(詳細は[規約とスタイル](conventions)を見てください)
<div class="original-doc">
:	This line declares our controller,  each controller class has to be prefixed with `Controller_` and an underscore delimited path to the folder the controller is in (see [Conventions and styles](about.conventions) for more info).  Each controller should also extend the base `Controller` class which provides a standard structure for controllers.
</div>

`public function action_index()`
: これはコントローラーに"index"アクションを定義しています。Kohanaはユーザがアクションを指定しない場合にこのアクションを呼び出します。
<div class="original-doc">
:	This defines the "index" action of our controller.  Kohana will attempt to call this action if the user hasn't specified an action. (See [Routes, URLs and Links](tutorials.urls))
</div>

`echo 'hello, world!';`
: そしてこれがお決まりの文句を出力する行です！

ブラウザを開き、http://localhost/index.php/helloにアクセスすれば、このような表示がされるはずです:
<div class="original-doc">
:	And this is the line which outputs the customary phrase!

Now if you open your browser and go to http://localhost/index.php/hello you should see something like:
</div>
![Hello, World!](hello_world_1.png "Hello, World!")

## それはよいが、もっと上手くやれる {#that-was-good-but-we-can-do-better}

上記の節で我々が行ったことは*非常に*基礎的なKohanaアプリケーションを作成するのがいかに簡単であるかの良い例でした。（実際これは非常に基礎的です。2度とこれを作ってはいけません！）

あなたがMVCについて聞いたことがあれば、コントローラーの中で内容を反復することがMVCの原則に厳密に反していることはおそらく気がついているでしょう。

MVCフレームワークでコーディングする適切な方法はアプリケーションの表示部を処理する_views_を使用することです。そしてコントローラーが行うべき最善のこと（リクエストフローを制御すること！）を可能にしてください。

オリジナルのコントローラーを少し変更してみましょう。

<div class="original-doc">
## That was good, but we can do better

What we did in the previous section was a good example of how easy it to create an *extremely* basic Kohana app. (In fact it's so basic, that you should never make it again!)

If you've ever heard anything about MVC you'll probably have realised that echoing content out in a controller is strictly against the principles of MVC.

The proper way to code with an MVC framework is to use _views_ to handle the presentation of your application, and allow the controller to do what it does best – control the flow of the request!

Lets change our original controller slightly:
</div>


    <?php defined('SYSPATH') OR die('No Direct Script Access');

	Class Controller_Hello extends Controller_Template
	{
		public $template = 'site';

		public function action_index()
		{
			$this->template->message = 'hello, world!';
		}
	}

`extends Controller_Template`
: 今度はTemplateコントローラーを拡張します。これはコントローラーからビューを使用するのをさらに便利にします。
<div class="original-doc">
:	We're now extending the template controller,  it makes it more convenient to use views within our controller.
</div>

`public $template = 'site';`
: Templateコントローラーは何のテンプレートを使いたいのか知る必要があります。この変数に定義されたビューを自動的にロードし、Viewオブジェクトをここにセットします。
<div class="original-doc">
:	The template controller needs to know what template you want to use. It'll automatically load the view defined in this variable and assign the view object to it.
</div>

`$this->template->message = 'hello, world!';`
:	`$this->template`はsiteテンプレート用のViewオブジェクトへの参照です。ここで行っていることは、"message"という変数にビューに表示する"Hello,World"の値を割り当てています。
視界に。

ではコードを走らせてみましょう。

<div class="original-doc">
:	`$this->template` is a reference to the view object for our site template.  What we're doing here is assigning a variable called "message", with a value of "hello, world!" to the view.

Now lets try running our code...
</div>

![Hello, World!](hello_world_2_error.png "Hello, World!")

ある理由でKohanaはヒステリーをお越し、私たちを驚かせるメッセージを表示しています。

エラーメッセージを見れば、私たちがまだsiteテンプレートを作成していないのでViewライブラリがそれを見つけられないのだということがおそらく分かるでしょう。
メッセージを表示するためにビューファイル `application/views/site.php` を作しましょう。
<div class="original-doc">
For some reason Kohana's thrown a wobbly and isn't showing our amazing message.

If we look at the error message we can see that the View library wasn't able to find our site template, probably because we haven't made it yet – *doh*!

Let's go and make the view file `application/views/site.php` for our message:
</div>

	<html>
		<head>
			<title>We've got a message for you!</title>
			<style type="text/css">
				body {font-family: Georgia;}
				h1 {font-style: italic;}

			</style>
		</head>
		<body>
			<h1><?php echo $message; ?></h1>
			<p>We just wanted to say it! :)</p>
		</body>
	</html>

ページを更新すれば労働の成果を見ることができます。

<div class="original-doc">
If we refresh the page then we can see the fruits of our labour:
</div>

![hello, world! We just wanted to say it!](hello_world_2.png "hello, world! We just wanted to say it!")

## ステージ3 {#stage-3}

このチュートリアルでは、コントローラーを作り、表示からロジックを分けるためにビューを使用する方法を学習しました。

これは明らかにKohanaの使い方の非常に基礎的な入門で、アプリケーションを開発する場合にKahanaの持っている可能性に触れさえしてません。

<div class="original-doc">
## Stage 3 – Profit!

In this tutorial you've learnt how to create a controller and use a view to separate your logic from your display.

This is obviously a very basic introduction to working with Kohana and doesn't even scrape the potential you have when developing applications with it.
</div>