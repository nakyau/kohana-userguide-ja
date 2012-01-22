# エラー処理/例外処理 {#error-exception-handling}

KohanaはPHPの[ErrorException](http://php.net/errorexception)クラスを利用して、エラーを例外に転換し、例外ハンドラとエラーハンドラの両方を提供します。エラーとアプリケーションの内部ステータスの詳細はハンドラによって表示されます。

1. Exception クラス
2. エラーレベル
3. エラーメッセージ
4. エラーのソースをハイライトと共に表示
5. 例外フローの[デバッグトレース](http://php.net/debug_backtrace) 
6. インクルードされたファイル、ロードされたエクステンション、グローバル変数

<div class="original-doc">
# Error/Exception Handling

Kohana provides both an exception handler and an error handler that transforms errors into exceptions using PHP's [ErrorException](http://php.net/errorexception) class. Many details of the error and the internal state of the application is displayed by the handler:

1. Exception class
2. Error level
3. Error message
4. Source of the error, with the error line highlighted
5. A [debug backtrace](http://php.net/debug_backtrace) of the execution flow
6. Included files, loaded extensions, and global variables
</div>

## 例 {#example}

リンクをクリックで追加の情報をトグル表示する:



<div class="original-doc">
## Example

Click any of the links to toggle the display of additional information:
</div>

<div class="original-doc">
	{{userguide/examples/error} }
</div>

## エラー処理/例外処理を無効にする {#disabling-error-exception-handling}

内部のエラー処理を表示したくなければ、[Kohana::init]を呼び出すときにその処理を無効にすることができます。

<div class="original-doc">
## Disabling Error/Exception Handling

If you do not want to use the internal error handling, you can disable it (highly discouraged) when calling [Kohana::init]:
</div>

    Kohana::init(array('errors' => FALSE));

## エラーリポート {#error-reporting}

デフォルトでは、KohanaはstrictモードのWarningを含む全てのエラーを表示します。これについては [error_reporting](http://php.net/error_reporting)を使用して設定します。

    error_reporting(E_ALL | E_STRICT);

あなたのアプリケーションが本番環境で公開されているのならば、このようにnoticeを無視するより保守的な設定が推奨されます。
    error_reporting(E_ALL & ~E_NOTICE);


エラーが起きたときに白い画面が表示されるのならば、おそらくあなたのホストはエラー表示を無効にしています。`error_reporting`呼び出しの後ろに以下の記述を加えることで、エラー表示をオンにできます。

    ini_set('display_errors', TRUE);

エラーが起きたとき、空白のページよりはましなエラーページを提供する[例外処理/エラー処理](debugging.errors)を使えるようになるので、公開時であってさえも、エラーは**常に**表示されているべきです。 

<div class="original-doc">
## Error Reporting

By default, Kohana displays all errors, including strict mode warnings. This is set using [error_reporting](http://php.net/error_reporting):

    error_reporting(E_ALL | E_STRICT);

When you application is live and in production, a more conservative setting is recommended, such as ignoring notices:

    error_reporting(E_ALL & ~E_NOTICE);

If you get a white screen when an error is triggered, your host probably has disabled displaying errors. You can turn it on again by adding this line just after your `error_reporting` call:

    ini_set('display_errors', TRUE);

Errors should **always** be displayed, even in production, because it allows you to use [exception and error handling](debugging.errors) to serve a nice error page rather than a blank white screen when an error happens.
</div>
## HTTP 例外処理 {#http-exception-handling}

KohanaにはHTTPエラーを処理するための強健なシステムが付属します。これには各HTTPステータスコードへの例外クラスを含みます。
アプリケーションにおいて404を起こすにはこのように書きます。

	throw new HTTP_Exception_404('File not found!');


Kohanaにはこれらのエラーを処理するデフォルトのメソッドはありません。この種のエラーを処理する例外ハンドラをあなた自身がセットアップ（して且つ登録）することが推奨されます。
これは*/application/classes/foobar/exception/handler.php*に入る簡単な例です。
<div class="original-doc">
## HTTP Exception Handling

Kohana comes with a robust system for handing http errors. It includes exception classes for each http status code. To trigger a 404 in your application (the most common scenario):

	throw new HTTP_Exception_404('File not found!');

There is no default method to handle these errors in Kohana. It's recommended that you setup an exception handler (and register it) to handle these kinds of errors. Here's a simple example that would go in */application/classes/foobar/exception/handler.php*:
</div>

	class Foobar_Exception_Handler
	{
		public static function handle(Exception $e)
		{
			switch (get_class($e))
			{
				case 'Http_Exception_404':
					$response = new Response;
					$response->status(404);
					$view = new View('error_404');
					$view->message = $e->getMessage();
					$view->title = 'File Not Found';
					echo $response->body($view)->send_headers()->body();
					return TRUE;
					break;
				default:
					return Kohana_Exception::handler($e);
					break;
			}
		}
	}


そして、ハンドラを登録するためにブートストラップにこのような記述を入れてください。

	set_exception_handler(array('Foobar_Exception_Handler', 'handle'));

 > *注意:* ブートストラップにおいて、`set_exception_handler()`の位置が`Kohana::init()`より**後に**あることを確認してください。そうでなければ、動作しないでしょう。
 
 > *Fatal error: Unkownの0行目でスタックフレームなしでスローされた例外*を受け取った場合、それは例外ハンドラでエラーが起きたことを意味します。上記の例を使用していれば、*/application/views/error/*の配下に*404.php*が存在することを確認してください。

<div class="original-doc">
And put something like this in your bootstrap to register the handler.

	set_exception_handler(array('Foobar_Exception_Handler', 'handle'));

 > *Note:* Be sure to place `set_exception_handler()` **after** `Kohana::init()` in your bootstrap, or it won't work.
 
 > If you receive *Fatal error: Exception thrown without a stack frame in Unknown on line 0*, it means there was an error within your exception handler. If using the example above, be sure *404.php* exists under */application/views/error/*.
</div>