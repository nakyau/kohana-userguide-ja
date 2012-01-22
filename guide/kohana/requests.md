# リクエスト {#requests}

Kohanaは柔軟なHMVCリクエストシステムを備えています。内部リクエストおよび外部リクエストをサポートします。

HMVS は`Hierarchical Model View Controller`の略であり、基本的にはリクエストは内部から呼び出される各MVCのトリオを持つことができます。

KohanaのリクエストオブジェクトはHTTP/1.1に準拠しています。
<div class="original-doc">
# Requests

Kohana includes a flexible HMVC request system. It supports out of the box support for internal requests and external requests.

HMVC stands for `Hierarchical Model View Controller` and basically means requests can each have MVC triads called from inside each other.

The Request object in Kohana is HTTP/1.1 compliant.
</div>
## リクエストの作成 {#creating-requests}

リクエストの作成はとても簡単です。

### 内部リクエスト {#internal-requests}

内部リクエストとは内部のアプリケーションに対してのリクエストです。渡されるURLに基づいたアプリケーションを指定するために、[ルート](routing)を使用します。
基本的な内部リクエストは以下のようなものです。
<div class="original-doc">
## Creating Requests

Creating a request is very easy:
</div>

<div class="original-doc">
### Internal Requests

An internal request is a request calling to the internal application. It utilizes [routes](routing) to direct the application based on the URI that is passed to it. A basic internal request might look something like:

	$request = Request::factory('welcome');

In this example, the URI is 'welcome'.
</div>

#### 初期リクエスト {#the-initial-request}

KohanaはHMVCを使用するので、内部で互いに多くのリクエストも呼び出すことができます。
最初のリクエスト（通常init.phpから呼び出される）は"初期リクエスト"と呼ばれます。初期リクエストにはこのようにアクセスできます。

<div class="original-doc">
#### The initial request

Since Kohana uses HMVC, you can call many requests inside each other. The first request (usually called from `index.php`) is called the "initial request". You can access this request via:

	Request::initial();

You should only use this method if you are absolutely sure you want the initial request. Otherwise you should use the `Request::current()` method.
</div>
#### サブリクエスト {#sub-requests}

`Request::factory()`を使ってアプリケーションに対して何度でもリクエストを呼び出せます。これら全てのリクエストはサブリクエストであると考えられるでしょう。

この違いの他は、全く同じです。is_initial()メソッドを使ってリクエストがサブリクエストであるかを検知できます。
<div class="original-doc">
#### Sub-requests

You can call a request at any time in your application by using the `Request::factory()` syntax. All of these requests will be considered sub-requests.

Other than this difference, they are exactly the same. You can detect if the request is a sub-request in your controller with the is_initial() method:

	$sub_request = ! $this->request->is_initial()
</div>
### 外部リクエスト {#external-requests}

外部リクエストはサードパーティのウェブサイトから呼び出されます。
外部リクエストは遠隔のサイトからHTMLをスクレイプするときや、サードパーティAPIに対してREST呼び出しを行うのに使用します。

	// GETを使用
	$request = Request::factory('http://www.google.com/');

	// PUTを使用
	$request = Request::factory('http://example.com/put_api')->method(Request::PUT)->body(json_encode('the body'))->headers('Content-Type', 'application/json');

	// POSTを使用
	$request = Request::factory('http://example.com/post_api')->method(Request::POST)->post(array('foo' => 'bar', 'bar' => 'baz'));

<div class="original-doc">
### External Requests

An external request calls out to a third party website.

You can use this to scrape HTML from a remote site, or make a REST call to a third party API:

	// This uses GET
	$request = Request::factory('http://www.google.com/');

	// This uses PUT
	$request = Request::factory('http://example.com/put_api')->method(Request::PUT)->body(json_encode('the body'))->headers('Content-Type', 'application/json');

	// This uses POST
	$request = Request::factory('http://example.com/post_api')->method(Request::POST)->post(array('foo' => 'bar', 'bar' => 'baz'));
</div>

## リクエストの実行 {#executing-requests}

リクエストを実行するにはリクエストオブジェクトの`excecute()`メソッドを使用します。execute()メソッドは[レスポンス](responses) オブジェクトを返します。

	$request = Request::factory('welcome');
	$response = $request->execute();

## リクエストキャッシュ制御

実行速度を速くするためのリクエストのキャッシュをfactoryメソッドの第2引数にキャッシュインスタンスを渡すことで行うことができます。

	$request = Request::factory('welcome', Cache::instance());

<div class="original-doc">
## Executing Requests

To execute a request, use the `execute()` method on it. This will give you a [response](responses) object.

	$request = Request::factory('welcome');
	$response = $request->execute();

## Request Cache Control

You can cache requests for fast execution by passing a cache instance in as the second parameter of factory:

	$request = Request::factory('welcome', Cache::instance());

</div>
TODO
