# コントローラー {#controllers}

コントローラーはモデルとビューの間に立つクラスファイルです。モデルによってデータが変更される必要があるときにその情報を渡したり、モデルからのデータを読み出すときに、読み出しの要求を行ないます。

コントローラーは[Request::execute()]によって[Route]に従ってURLにマッチする関数が呼び出されます。URLがどのようにコントローラーへ対応づけられるのかを理解するためには[routing](routing)ページを確認してください。

<div class="original-doc">
# Controllers

A Controller is a class file that stands in between the models and the views in an application. It passes information on to the model when data needs to be changed and it requests information from the model when data needs to be loaded. Controllers then pass on the information of the model to the views where the final output can be rendered for the users.  Controllers essentially control the flow of the application.

Controllers are called by the [Request::execute()] function based on the [Route] that the url matched.  Be sure to read the [routing](routing) page to understand how to use routes to map urls to your controllers.
</div>

## コントローラーの作成 {#creating-controllers}

コントローラーが機能するために以下を満たさねばなりません:

* classes/controller（またはそのサブディレクトリ）に属すること
* ファイル名は小文字（例、articles.php）
* クラス名が（_を/に置き換えた）ファイル名に対応していること、且つ_以外の語の先頭は大文字であること
* Controllerクラスを親クラスに持つこと

コントローラークラスの名前とファイルパスの例:
<div class="original-doc">
## Creating Controllers

In order to function, a controller must do the following:

* Reside in `classes/controller` (or a sub-directory)
* Filename must be lowercase, e.g. `articles.php`
* The class name must map to the filename (with `/` replaced with `_`) and each word is capitalized
* Must have the Controller class as a (grand)parent

Some examples of controller names and file locations:
</div>

	// classes/controller/foobar.php
	class Controller_Foobar extends Controller {
	
	// classes/controller/admin.php
	class Controller_Admin extends Controller {

コントローラーはサブディレクトリに置くこともできます:
<div class="original-doc">
Controllers can be in sub-folders:
</div>

	// classes/controller/baz/bar.php
	class Controller_Baz_Bar extends Controller {
	
	// classes/controller/product/category.php
	class Controller_Product_Category extends Controller {

[!!] サブフォルダ中のコントローラーはデフォルトのルートからは呼び出されないことに注意してください。 [Routingのdirectory](routing#directory) パラメータまたはディレクトリのデフォルト値をルートに定義する必要があります。

コントローラーはほかのクラスから拡張できます。
<div class="original-doc">	
[!!] Note that controllers in sub-folders can not be called by the default route, you will need to define a route that has a [directory](routing#directory) param or sets a default value for directory.

Controllers can extend other controllers.
</div>

	// classes/controller/users.php
	class Controller_Users extends Controller_Template
	
	// classes/controller/api.php
	class Controller_Api extends Controller_REST

[!!] [Controller_Template] と [Controller_REST] はKohanaで提供されているコントローラーの例です。

すべてのコントローラーでログインが必要なときなど、コントローラー間で処理を共有するのに、ほかのコントローラーから拡張することができます。
<div class="original-doc">	
[!!] [Controller_Template] is an example controller provided in Kohana.

You can also have a controller extend another controller to share common things, such as requiring you to be logged in to use all of those controllers.
</div>

	// classes/controller/admin.php
	class Controller_Admin extends Controller {
		// This controller would have a before() that checks if the user is logged in
	
	// classes/controller/admin/plugins.php
	class Controller_Admin_Plugins extends Controller_Admin {
		// Because this controller extends Controller_Admin, it would have the same logged in check

## $this->request

各コントローラーはコントローラによって呼び出される[Request]オブジェクトである`$this->request`プロパティを持っています。
これを使って`$this->response->body($ouput)を経由してレスポンスをbodyにセットするのと同様に、`現在のリクエストについての情報を得ることができます。

ここに`$this->request`で利用可能なプロパティとメソッドのリストの一部を示します。 `Request::instance()`からでもアクセスできますが、`$this->request`はショートカットとして提供します。
このことに関するさらなる情報は [Request] を見てください。

Property/method | 動作
--- | ---
[$this->request->route()](../api/Request#property:route) | 現在のリクエストURLにマッチする[Route]
[$this->request->directory()](../api/Request#property:directory), <br /> [$this->request->controller](../api/Request#property:controller), <br /> [$this->request->action](../api/Request#property:action) | 現在のルートにマッチするディレクトリ、コントローラーとアクション
[$this->request->param()](../api/Request#param) | ルートに定義されたその他のパラメータ
[$this->request->redirect()](../api/Request#redirect) | 別のURLにリクエストをリダイレクトする

<div class="original-doc">		
Every controller has the `$this->request` property which is the [Request] object that called the controller.  You can use this to get information about the current request, as well as set the response body via `$this->response->body($ouput)`.

Here is a partial list of the properties and methods available to `$this->request`.  These can also be accessed via `Request::instance()`, but `$this->request` is provided as a shortcut.  See the [Request] class for more information on any of these. 

Property/method | What it does
--- | ---
[$this->request->route()](../api/Request#property:route) | The [Route] that matched the current request url
[$this->request->directory()](../api/Request#property:directory), <br /> [$this->request->controller](../api/Request#property:controller), <br /> [$this->request->action](../api/Request#property:action) | The directory, controller and action that matched for the current route
[$this->request->param()](../api/Request#param) | Any other params defined in your route
[$this->request->redirect()](../api/Request#redirect) | Redirect the request to a different url
</div>

## $this->response
Method | 動作
--- | ---
[$this->response->body()](../api/Response#property:body) | リクエストに内容を返す
[$this->response->status()](../api/Response#property:status) | リクエストのHTTPステータス (200, 404, 500, etc.)
[$this->response->headers()](../api/Response#property:headers) | レスポンスとともに返すHTTPヘッダー
<div class="original-doc">

[$this->response->body()](../api/Response#property:body) | The content to return for this request
[$this->response->status()](../api/Response#property:status) | The HTTP status for the request (200, 404, 500, etc.)
[$this->response->headers()](../api/Response#property:headers) | The HTTP headers to return with the response
</div>

## アクション {#actions}

関数名に`action_` プレフィックスを付けてpublicの関数を定義することでコントローラーにアクションを作成できます。 `public`で定義されていない関数と`action_`プレフィックスがついていない関数はルーティング経由で呼び出しできません。

アクションメソッドはリクエストに応じて何を行なうべきかを判定し、アプリケーションを制御します。ユーザがブログの記事を保存したい？必須の項目を提供したか？その権限はあるか？コントローラーはこれを成し遂げるためにモデルを含むほかのクラスを呼び出します。スクリプトの修了までに[リダイレクト](../api/Request#redirect) が行なわれる場合を除いて、各アクションは[ビューファイル](mvc/views)がブラウザに送られるように`$this->response->body($view)`をセットしなくてはなりません。

[view](mvc/views) をロードする非常に基本的なアクションメソッド:

	public function action_hello()
	{
		$this->response->body(View::factory('hello/world')); // /hello/world.phpをロードします
	}

<div class="original-doc">
## Actions

You create actions for your controller by defining a public function with an `action_` prefix.  Any method that is not declared as `public` and prefixed with `action_` can NOT be called via routing.

An action method will decide what should be done based on the current request, it *controls* the application.  Did the user want to save a blog post?  Did they provide the necessary fields?   Do they have permission to do that?  The controller will call other classes, including models, to accomplish this.  Every action should set `$this->response->body($view)` to the [view file](mvc/views) to be sent to the browser, unless it [redirected](../api/Request#redirect) or otherwise ended the script earlier.

A very basic action method that simply loads a [view](mvc/views) file.

	public function action_hello()
	{
		$this->response->body(View::factory('hello/world')); // This will load views/hello/world.php
	}
</div>


### パラメータ {#parameters}

パラメータには `$this->request->param('name')`を呼び出すことでアクセスできます。 `name`の部分にはRouteで指定した名前が入ります。

	// ルートの設定がこうなっているとき→ Route::set('example','<controller>(/<action>(/<id>(/<new>)))');
	
	public function action_foobar()
	{
		$id = $this->request->param('id');
		$new = $this->request->param('new');

<div class="original-doc">
### Parameters

Parameters are accessed by calling `$this->request->param('name')` where `name` is the name defined in the route.

	// Assuming Route::set('example','<controller>(/<action>(/<id>(/<new>)))');
	
	public function action_foobar()
	{
		$id = $this->request->param('id');
		$new = $this->request->param('new');

</div>

パラメータがセットされていなければNULLが返ります。第２引数にパラメータがセットされていなかった場合のデフォルト値を指定できます。

<div class="original-doc">
If that parameter is not set it will be returned as NULL.  You can provide a second parameter to set a default value if that param is not set.
</div>

	public function action_foobar()
	{
		// $id will be false if it was not supplied in the url
		$id = $this->request->param('user',FALSE);

### 例 {#examples}

公開ページのアクション:
<div class="original-doc">
### Examples

A view action for a product page.
</div>

	public function action_view()
	{
		$product = new Model_Product($this->request->param('id'));

		if ( ! $product->loaded())
		{
			throw new HTTP_Exception_404('Product not found!');
		}

		$this->response->body(View::factory('product/view')
			->set('product', $product));
	}

ユーザログインのアクション:
<div class="original-doc">
A user login action.
</div>
	public function action_login()
	{
		$view = View::factory('user/login');

		if ($_POST)
		{
			// Try to login
			if (Auth::instance()->login(arr::get($_POST, 'username'), arr::get($_POST, 'password')))
			{
				Request::current()->redirect('home');
			}

			$view->errors = 'Invalid email or password';
		}

		$this->response->body($view);
	}

## before と after {#before-and-after}

アクションが実行される前とアクションが実行された後に呼び出されるコードを記述するために `before()` と `after()` 関数が使えます。
例えば、ユーザがログインしているかをチェックしてからテンプレートビューを設定し、必要なファイルをロードするときなどです。

アクションがリクエストされて(`$this->request->action`を経由) ユーザがログインしていることが要求される処理をおこなうときに、ユーザがログインしていなければそのことをチェックすることが出来ます。

	// beforeでログインをチェックし、必要ならばリダイレクトする

	Controller_Admin extends Controller {

		public function before()
		{
			// If this user doesn't have the admin role, and is not trying to login, redirect to login
			if ( ! Auth::instance()->logged_in('admin') AND $this->request->action !== 'login')
			{
				$this->request->redirect('admin/login');
			}
		}
		
		public function action_login() {
			...


<div class="original-doc">
## Before and after

You can use the `before()` and `after()` functions to have code executed before or after the action is executed. For example, you could check if the user is logged in, set a template view, loading a required file, etc.

For example, if you look in `Controller_Template` you can see that in the be

You can check what action has been requested (via `$this->request->action`) and do something based on that, such as requiring the user to be logged in to use a controller, unless they are using the login action.

	// Checking auth/login in before, and redirecting if necessary:

	Controller_Admin extends Controller {

		public function before()
		{
			// If this user doesn't have the admin role, and is not trying to login, redirect to login
			if ( ! Auth::instance()->logged_in('admin') AND $this->request->action !== 'login')
			{
				$this->request->redirect('admin/login');
			}
		}
		
		public function action_login() {
			...

</div>
### カスタムコンストラクター {#custom-construct-function}

通常は、必要な動作を`before()`の中で完了するようにすれば`__construct()`を変更する必要はありません。
もしコンストラクタを修正するのならば、引数をそのままに残さなければなりません。そうであればコントローラからコールされるRequestオブジェクトは利用可能です。
もう一度伝えてきますが、ほとんどのケースでは`before()`を使い、コンストラクターは変更しないようにするべきます。
しかし、どうしても本当に必要ならばこのようにしてください:

	// このようなことはほとんど必要ありません。 かわりに before() を使うこと！

	// パラメータの Kohana_Request を確認する
	public function __construct(Request $request, Response $response)
	{
		// parent::__construct をコンストラクタ中のどこかで呼び出すこと
		parent::__construct($request, $response);
		
		// ここに必要な処理を書く
	}

<div class="original-doc">
### Custom __construct() function

In general, you should not have to change the `__construct()` function, as anything you need for all actions can be done in `before()`.  If you need to change the controller constructor, you must preserve the parameters or PHP will complain.  This is so the Request object that called the controller is available.  *Again, in most cases you should probably be using `before()`, and not changing the constructor*, but if you really, *really* need to it should look like this:

	// You should almost never need to do this, use before() instead!

	// Be sure Kohana_Request is in the params
	public function __construct(Request $request, Response $response)
	{
		// You must call parent::__construct at some point in your function
		parent::__construct($request, $response);
		
		// Do whatever else you want
	}
</div>

## ほかのコントローラーを拡張する {#extending-other-controllers}
<div class="original-doc">
## Extending other controllers
</div>
TODO: More description and examples of extending other controllers, multiple extension, etc.
