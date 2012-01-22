# ルーティング {#routing}

Kohanaは非常に強力なルーティングシステムを提供します。本質的には、ルートはURLとコントローラー間のインターフェースおよびアクションを提供します。
正確なルートでほとんどのURLスキームを任意のコントローラー配置に一致させることができるはずです。
また、他方に影響を与えず一方を変更することもできます。

[Request Flow](flow)セクションで言及されているように、リクエストは[Request]クラスによって処理されます。それらは一致する[Route]を探し、そのリクエストを処理するために適切なコントローラーをロードします。


[!!] **ルートはルートが追加された順にマッチする**ことを理解することが重要です。またルートがURLとマッチすれば直ちに、ルーティング処理は本質的に停止され、 *残りのルートに対してルーティング処理は実行されません*。
デフォルトのルートは空のURLも含めほとんどのURLにマッチするので、新しいルートはデフォルトルートより前に置かれるでしょう。

<div class="original-doc">
# Routing

Kohana provides a very powerful routing system.  In essence, routes provide an interface between the urls and your controllers and actions.  With the correct routes you could make almost any url scheme correspond to almost any arrangement of controllers, and you could change one without impacting the other.

As mentioned in the [Request Flow](flow) section, a request is handled by the [Request] class, which will look for a matching [Route] and load the appropriate controller to handle that request.

[!!] It is important to understand that **routes are matched in the order they are added**, and as soon as a URL matches a route, routing is essentially "stopped" and *the remaining routes are never tried*.  Because the default route matches almost anything, including an empty url, new routes must be place before it.
</div>
## ルートの作成 {#creating-routes}

`APPPATH/bootstrap.php`を見れば、以下の "default"ルートがあるでしょう:
<div class="original-doc">
## Creating routes

If you look in `APPPATH/bootstrap.php` you will see the "default" route as follows:
</div>

	Route::set('default', '(<controller>(/<action>(/<id>)))')
	->defaults(array(
		'controller' => 'welcome',
		'action'     => 'index',
	));
	
[!!] デフォルトのルートは単にサンプルとして提供されています。これを削除または置き換えてあなたのルートを作成できます。

 `default` という名前で `(<controller>(/<action>(/<id>)))`形式のURLにマッチするルートを造ります。.  

[Route::set]の各パラメーターを詳しくみてみましょう。 `name`, `uri`とオプションの配列`regex`があります。

<div class="original-doc">
[!!] The default route is simply provided as a sample, you can remove it and replace it with your own routes.

So this creates a route with the name `default` that will match urls in the format of `(<controller>(/<action>(/<id>)))`.  

Let's take a closer look at each of the parameters of [Route::set], which are `name`, `uri`, and an optional array `regex`.
</div>
### ルート名 {#name}

ルートの名前は**一意な**文字列である必要があります。もしそうでなければ同じ名前の古いルートを上書きします。名前はリバースルーティングによってURLを作成する、またはルートが一致したかをチェックするために使用されます。

<div class="original-doc">
### Name

The name of the route must be a **unique** string.  If it is not it will overwrite the older route with the same name. The name is used for creating urls by reverse routing, or checking which route was matched.
</div>


### URI
URIは一致すべきURLの書式を表す文字列です。`<>`で囲まれたトークンは *キー* と `()`で囲まれたURIのオプションパーツです。 Kohana のルートにおいて、どんな文字も許可され、`()<>`の外側はリテラルとして扱われます。`/`はURLの一部と一致するであろうこと意外に意味を持ちません。通常`/` は静的なセパレータとして使用されますが、regex中でも意味を持ち、ルートにどのような形式をもたせるかということに制限はありません。

もう一度デフォルトのルートを見てみましょう。そのURLは`(<controller>(/<action>(/<id>)))`です。3つのキーないしパラメータ、controller,action,idがあります。
この場合は、全てのURIはオプションです。従ってブランクのURLも一致します。また（defaults()によってセットされた）デフォルトコントローラーとアクションは`Controller_Welcome`クラスがロードされ、`action_index`メドッドがリクエストを処理するして結果を返すことが想定されます。

キーにはどんな名前も使用できますが、以下のキーは[Request]オブジェクトへの特別な意味を持ちます。また影響するコントローラーとアクションはそれらを呼び出します:

 * **Directory** - `classes/controller`のサブディレクトリのクラスを探すため (\[covered below]\(#directory))
 * **Controller** - リクエストが処理すべきコントローラー
 * **Action** - 呼び出すアクション名
<div class="original-doc">
The uri is a string that represents the format of urls that should be matched.  The tokens surrounded with `<>` are *keys* and anything surrounded with `()` are *optional* parts of the uri. In Kohana routes, any character is allowed and treated literally aside from `()<>`.  The `/` has no meaning besides being a character that must match in the uri.  Usually the `/` is used as a static seperator but as long as the regex makes sense, there are no restrictions to how you can format your routes.

Lets look at the default route again, the uri is `(<controller>(/<action>(/<id>)))`.  We have three keys or params: controller, action, and id.   In this case, the entire uri is optional, so a blank uri would match and the default controller and action (set by defaults(), [covered below](#defaults)) would be assumed resulting in the `Controller_Welcome` class being loaded and the `action_index` method being called to handle the request.

You can use any name you want for your keys, but the following keys have special meaning to the [Request] object, and will influence which controller and action are called:

 * **Directory** - The sub-directory of `classes/controller` to look for the controller (\[covered below]\(#directory))
 * **Controller** - The controller that the request should execute.
 * **Action** - The action method to call.
</div>

### Regex

Kohanaのルートシステムはマッチング処理で [Perl互換の正規表現](http://perldoc.perl.org/perlre.html) を使用します。
デフォルトの各(`<>`で囲まれた)キーは`[^/.,;?\n]++` (英語で云うならば: anything that is not a slash, period, comma, semicolon, question mark, or newline)にマッチします。 
各キーにキーの連想配列と省略可能な第3の引数としてパターンを与えることで、独自のパターンを定義できます。

この例では、2つのディレクトリ`admin`と`affiliate`にコントローラーがあります。
このルートは`admin`または`affiliate`で始まるURLにだけ一致するので、デフォルトルートはまだ`classes/controller`で動作します。
<div class="original-doc">
The Kohana route system uses [perl compatible regular expressions](http://perldoc.perl.org/perlre.html) in its matching process.  By default each key (surrounded by `<>`) will match `[^/.,;?\n]++` (or in english: anything that is not a slash, period, comma, semicolon, question mark, or newline).  You can define your own patterns for each key by passing an associative array of keys and patterns as an additional third argument to Route::set.

In this example, we have controllers in two directories, `admin` and `affiliate`.  Because this route will only match urls that begin with `admin` or `affiliate`, the default route would still work for controllers in `classes/controller`.  
</div>

	Route::set('sections', '<directory>(/<controller>(/<action>(/<id>)))',
		array(
			'directory' => '(admin|affiliate)'
		))
		->defaults(array(
			'controller' => 'home',
			'action'     => 'index',
		));

さらに制限の無いパラメータと一致する、あるいはルートのオーバーフローを無視するためにあまり限定的でないregexを使用することができます。
この例でURL`foobar/baz/and-anything/else_that/is-on-the/url`は`Controller_Foobar::action_baz()`にルーティングされます。 そして `"stuff"` パラメータは `"and-anything/else_that/is-on-the/url"`になります。これを無制限のパラメータに使いたいなら、[explode](http://php.net/manual/en/function.explode.php) できます。またはただ余分を無視することもできます。

<div class="original-doc">
You can also use a less restrictive regex to match unlimited parameters, or to ignore overflow in a route.  In this example, the url `foobar/baz/and-anything/else_that/is-on-the/url` would be routed to `Controller_Foobar::action_baz()` and the `"stuff"` parameter would be `"and-anything/else_that/is-on-the/url"`.  If you wanted to use this for unlimited parameters, you could [explode](http://php.net/manual/en/function.explode.php) it, or you just ignore the overflow.  
</div>

	Route::set('default', '(<controller>(/<action>(/<stuff>)))', array('stuff' => '.*'))
		->defaults(array(
			'controller' => 'welcome',
			'action' => 'index',
	  ));


### 規定値 {#default-values}

ルート中のキーがオプション（またはルートに現れていない）ならば、キーの連想配列と規定値をもつ連想配列を[Route::defaults]に渡すことで、そのキーに規定値を与えることができます。
これはサイトにデフォルトコントローラーやアクションを提供するのに特に便利です。


[!!] `controller`と`action`キーはいつも値を持たなくてはなりません。 ですから、ルートにおいて要求（括弧の中ではなく）されるか、既定値を設定する必要があります。

デフォルトのルートにおいては全てのキーはオプションであり、コントローラーとアクションはデフォルト値を与えられています。もし空のURLを呼び出せば、`Controller_Welcome::action_index()`が規定値となり呼び出されるでしょう。 `foobar` を呼び出せば、アクションにだけ規定値が使用されて、 `Controller_Foobar::action_index()`を呼び出します。 最後に、`foobar/baz`を呼び出せば、どちらにも規定値は使われず、 `Controller_Foobar::action_baz()` が呼び出されます。

TODO: need an example here

You can also use defaults to set a key that isn't in the route at all.
ルートにまったく無いキーに規定値を設定することもできます。

TODO: example of either using directory or controller where it isn't in the route, but set by defaults
<div class="original-doc">
### Default values

If a key in a route is optional (or not present in the route), you can provide a default value for that key by passing an associated array of keys and default values to [Route::defaults], chained after your [Route::set].  This can be useful to provide a default controller or action for your site, among other things.

[!!] The `controller` and `action` key must always have a value, so they either need to be required in your route (not inside of parentheses) or have a default value provided.

In the default route, all the keys are optional, and the controller and action are given a default.   If we called an empty url, the defaults would fill in and `Controller_Welcome::action_index()` would be called.  If we called `foobar` then only the default for action would be used, so it would call `Controller_Foobar::action_index()` and finally, if we called `foobar/baz` then neither default would be used and `Controller_Foobar::action_baz()` would be called.

TODO: need an example here

You can also use defaults to set a key that isn't in the route at all.

TODO: example of either using directory or controller where it isn't in the route, but set by defaults
</div>

### ディレクトリ {#directory}

## ラムダ/コールバック ルート {#lambda-callbak-route-logic}


3.1ではラムダルートを使用することで高度なルーティングスキームを指定することができます。URIの代わりにルートを処理する関数へのコールバックや匿名関数を指定することもできます。
簡単な例を示します:


ラムダルートによるリバースルーティングを使いたいなら、第3引数に指定します
<div class="original-doc">
### Directory

## Lambda/Callback route logic

In 3.1, you can specify advanced routing schemes by using lambda routes. Instead of a URI, you can use an anonymous function or callback syntax to specify a function that will process your routes. Here's a simple example:

If you want to use reverse routing with lambda routes, you must pass the third parameter:
</div>
	Route::set('testing', function($uri)
		{
			if ($uri == 'foo/bar')
				return array(
					'controller' => 'welcome',
					'action'     => 'foobar',
				);
		},
		'foo/bar'
	);

以下のルートに見られるように、リバースURIパラメータは意味を成さないかもしれません。
<div class="original-doc">
As you can see in the below route, the reverse uri parameter might not make sense.
</div>

	Route::set('testing', function($uri)
		{
			if ($uri == '</language regex/>(.+)')
			{
				Cookie::set('language', $match[1]);
				return array(
					'controller' => 'welcome',
					'action'     => 'foobar'
				);
			}
		},
		'<language>/<rest_of_uri>
	);

php5.2を使用しているなら、この動作のために、今まで通りコールバックを使用してもよい。（この例はリバースルートを省略します）
<div class="original-doc">
If you are using php 5.2, you can still use callbacks for this behavior (this example omits the reverse route):
</div>

	Route::set('testing', array('Class', 'method_to_process_my_uri'));

## 例 {#examples}

ルートには数えきれないほどの可能性があります。ここにさらにいくつかの例をあげます:
<div class="original-doc">
## Examples

There are countless other possibilities for routes. Here are some more examples:
</div>

    /*
     * Authentication shortcuts
     */
    Route::set('auth', '<action>',
      array(
        'action' => '(login|logout)'
      ))
      ->defaults(array(
        'controller' => 'auth'
      ));
      
    /*
     * Multi-format feeds
     *   452346/comments.rss
     *   5373.json
     */
    Route::set('feeds', '<user_id>(/<action>).<format>',
      array(
        'user_id' => '\d+',
        'format' => '(rss|atom|json)',
      ))
      ->defaults(array(
        'controller' => 'feeds',
        'action' => 'status',
      ));
    
    /*
     * Static pages
     */
    Route::set('static', '<path>.html',
      array(
        'path' => '[a-zA-Z0-9_/]+',
      ))
      ->defaults(array(
        'controller' => 'static',
        'action' => 'index',
      ));
      
    /*
     * You don't like slashes?
     *   EditGallery:bahamas
     *   Watch:wakeboarding
     */
    Route::set('gallery', '<action>(<controller>):<id>',
      array(
        'controller' => '[A-Z][a-z]++',
        'action'     => '[A-Z][a-z]++',
      ))
      ->defaults(array(
        'controller' => 'Slideshow',
      ));
      
    /*
     * Quick search
     */
    Route::set('search', ':<query>', array('query' => '.*'))
      ->defaults(array(
        'controller' => 'search',
        'action' => 'index',
      ));

## リクエストパラメータ {#request-parameters}
`directory`、`controller`と`action`はこのように[Request]からパブリックプロパティでアクセスできます。:

<div class="original-doc">
## Request parameters

The `directory`, `controller` and `action` can be accessed from the [Request] as public properties like so:
</div>

	// From within a controller:
	$this->request->action();
	$this->request->controller();
	$this->request->directory();
	
	// Can be used anywhere:
	Request::current()->action();
	Request::current()->controller();
	Request::current()->directory();

ルートで指定したほかのキーは全て [Request::param()]を通じてアクセスできます:
<div class="original-doc">
All other keys specified in a route can be accessed via [Request::param()]:
</div>

	// From within a controller:
	$this->request->param('key_name');
	
	// Can be used anywhere:
	Request::current()->param('key_name');

[Request::param]メソッドはルートによってキーの値がセットされていなかった場合に規定値を返すために、省略可能な第2引数をとります。
もし引数が与えられなければ、全てのキーは連想配列を返します。
さらに `action`と`controller`、`directory`は [Request::param()]からアクセスすることができません。
例えば、以下のルートでは:

<div class="original-doc">
The [Request::param] method takes an optional second argument to specify a default return value in case the key is not set by the route. If no arguments are given, all keys are returned as an associative array.  In addition, `action`, `controller` and `directory` are not accessible via [Request::param()].

For example, with the following route:
</div>

	Route::set('ads','ad/<ad>(/<affiliate>)')
	->defaults(array(
		'controller' => 'ads',
		'action'     => 'index',
	));
	
ルートがURLに一致したら、`Controller_Ads::index()`が呼ばれます。そのコントローラーの[Request]オブジェクトの`param()`メソッドを使ってパラメータにアクセスできます。
`->defaults()`の中以外では、規定値を定義する ([Request::param]の省略可能な第2引数を通じて) ことを忘れないでください。
<div class="original-doc">
If a url matches the route, then `Controller_Ads::index()` will be called.  You can access the parameters by using the `param()` method of the controller's [Request]. Remember to define a default value (via the second, optional parameter of [Request::param]) if you didn't in `->defaults()`.
</div>

	class Controller_Ads extends Controller {
		public function action_index()
		{
			$ad = $this->request->param('ad');
			$affiliate = $this->request->param('affiliate',NULL);
		}
	
## ルートを定義すべきところは？ {#where-should-routes-be-defined}

確立しているルールはルートがモジュールに属するときはカスタムルートを`MODPATH/<module>/init.php`に置き、アプリケーション特有のルートは単に`APPPATH/bootstrap.php`ファイルに追加する（上記の**デフォルトルート**を確認してください）ことです。
当然ながら、ルートを外部ファイルからインクルードすることや動的に生成することも可能です。

<div class="original-doc">
## Where should routes be defined?

The established convention is to either place your custom routes in the `MODPATH/<module>/init.php` file of your module if the routes belong to a module, or simply insert them into the `APPPATH/bootstrap.php` file (be sure to put them **above** the default route) if they are specific to the application. Of course, nothing stops you from including them from an external file, or even generating them dynamically.
</div>

## A deeper look at how routes work

TODO: talk about how routes are compiled

## ルートを使用してURLとリンクを作成する {#creating-urls-and-links-using-routes}

Kohanaの強力なルーティング能力はルートのURIへのURLを生成するいくつかの方法をメソッドを備えています。
このようにURLを作成する[URL::site] を使って、いつでもサイトのURIを指定できます。
<div class="original-doc">
## Creating URLs and links using routes

Along with Kohana's powerful routing capabilities are included some methods for generating URLs for your routes' uris. You can always specify your uris as a string using [URL::site] to create a full URL like so:
</div>

    URL::site('admin/edit/user/'.$user_id);

Kohanaはルートの定義からURLを生成するメソッドも提供します。これはURIを文字列として指定したコードを変更することによって、ルーティングがいつか変わってしまうかもしれないときに特に有用です。ここに動的に生成する`feed`ルートに一致する例があります。

<div class="original-doc">
However, Kohana also provides a method to generate the uri from the route's definition. This is extremely useful if your routing could ever change since it would relieve you from having to go back through your code and change everywhere that you specified a uri as a string. Here is an example of dynamic generation that corresponds to the `feeds` route example from above:
</div>

    Route::get('feeds')->uri(array(
      'user_id' => $user_id,
      'action' => 'comments',
      'format' => 'rss'
    ));

その後にそのルートを`feeds/<user_id>(/<action>).<format>`に変更することでさらにルート定義を冗長にすることに決めたとしましょう。もし上記のURI生成メソッドで書いていれば1行も変更する必要は無いでしょう！
URIの一部が括弧で囲まれ、それらがURI生成のための値を持たず、デフォルト値もせっていされていないキーを指定する場合、その部分はURIから取り除かれます。この例はidが提供されないときデフォルトルートの`(/<id>)`部分は生成URIに含まれないということです。

頻繁に利用するメソッドの一つに現在のルート、ディレクトリ、コントローラーおよびアクションを想定すること以外は上記と同じである[Request::uri]があります。

<div class="original-doc">
Let's say you decided later to make that route definition more verbose by changing it to `feeds/<user_id>(/<action>).<format>`. If you wrote your code with the above uri generation method you wouldn't have to change a single line! When a part of the uri is enclosed in parentheses and specifies a key for which there in no value provided for uri generation and no default value specified in the route, then that part will be removed from the uri. An example of this is the `(/<id>)` part of the default route; this will not be included in the generated uri if an id is not provided.

One method you might use frequently is the shortcut [Request::uri] which is the same as the above except it assumes the current route, directory, controller and action. If our current route is the default and the uri was `users/list`, we can do the following to generate uris in the format `users/view/$id`:
</div>

    $this->request->uri(array('action' => 'view', 'id' => $user_id));
    
ビューの中で、好ましい方法は:
<div class="original-doc">
Or if within a view, the preferable method is:
</div>

    Request::instance()->uri(array('action' => 'view', 'id' => $user_id));

TODO: examples of using html::anchor in addition to the above examples

## Testing routes

TODO: mention bluehawk's devtools module