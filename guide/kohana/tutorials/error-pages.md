# 親切なエラーページ {#friendly-error-pages}
Kohana 3ではデフォルトでKohana 2にあったような親切なエラーページを表示しません。
この短いガイドで親切なエラーページを表示する方法を学びます。

<div class="original-doc">
# Friendly Error Pages

By default Kohana 3 doesn't have a method to display friendly error pages like that
seen in Kohana 2; In this short guide you will learn how it is done.
</div>

## 前提条件 {#prerequisites}

`Kohana::init`に`'errors' => TRUE`のパラメータを与える必要があります。
これはPHPのエラーをより扱いやすい例外に変換します。

<div class="original-doc">
## Prerequisites

You will need `'errors' => TRUE` passed to `Kohana::init`. This will convert PHP
errors into exceptions which are easier to handle.
</div>

## 1. 改善された例外ハンドラ {#an-improved-exception-handler}

我々のカスタム例外ハンドラは一目瞭然です。

<div class="original-doc">
## 1. An Improved Exception Handler

Our custom exception handler is self explanatory.
</div>

	class Kohana_Exception extends Kohana_Kohana_Exception {

		public static function handler(Exception $e)
		{
			if (Kohana::DEVELOPMENT === Kohana::$environment)
			{
				parent::handler($e);
			}
			else
			{
				try
				{
					Kohana::$log->add(Log::ERROR, parent::text($e));

					$attributes = array
					(
						'action'  => 500,
						'message' => rawurlencode($e->getMessage())
					);

					if ($e instanceof HTTP_Exception)
					{
						$attributes['action'] = $e->getCode();
					}

					// Error sub-request.
					echo Request::factory(Route::get('error')->uri($attributes))
					->execute()
					->send_headers()
					->body();
				}
				catch (Exception $e)
				{
					// Clean the output buffer if one exists
					ob_get_level() and ob_clean();

					// Display the exception text
					echo parent::text($e);

					// Exit with an error status
					exit(1);
				}
			}
		}
	}


開発環境では例外を渡し、そうでなければ

* エラーを記録
* ルートアクションとメッセージ属性をセット
* `HTTP_Exception`がthrowされたらアクションをエラーコードで上書き
* 内部サブリクエストを実行

アクションはHTTP応答コードとして使用されます。デフォルトでは500（Internal Server Error）で、そうでないならば`HTTP_Response_Exception`が投げられます。

<div class="original-doc">
If we are in the development environment then pass it off to Kohana otherwise:

* Log the error
* Set the route action and message attributes.
* If a `HTTP_Exception` was thrown, then override the action with the error code.
* Fire off an internal sub-request.

The action will be used as the HTTP response code. By default this is: 500 (internal
server error) unless a `HTTP_Response_Exception` was thrown.

So this:
</div>

	throw new HTTP_Exception_404(':file does not exist', array(':file' => 'Gaia'));

素敵な404エラーページを表示するには
<div class="original-doc">
would display a nice 404 error page, where:
</div>

	throw new Kohana_Exception('Directory :dir must be writable',
				array(':dir' => Debug::path(Kohana::$cache_dir)));


500エラーページを表示するには
<div class="original-doc">
would display an error 500 page.
</div>
**The Route**

	Route::set('error', 'error/<action>(/<message>)', array('action' => '[0-9]++', 'message' => '.+'))
	->defaults(array(
		'controller' => 'error_handler'
	));

## 2. エラーページ・コントローラー {#the-error-page-controller}
<div class="original-doc">
## 2. The Error Page Controller
</div>
	public function before()
	{
		parent::before();

		$this->template->page = URL::site(rawurldecode(Request::$initial->uri()));

		// Internal request only!
		if (Request::$initial !== Request::$current)
		{
			if ($message = rawurldecode($this->request->param('message')))
			{
				$this->template->message = $message;
			}
		}
		else
		{
			$this->request->action(404);
		}

		$this->response->status((int) $this->request->action());
	}

1. ユーザが何をリクエストしたかが解るようにテンプレートの変数 "page" に値をセットしてください。これは表示する目的だけのためにあります。
   
2. 内部リクエストであれば、テンプレート変数"message"にユーザに表示されるメッセージをセットします。
3. そうでなければ404アクションを使用します。ユーザは独自のエラーメッセージを作成できます。その例は
   `error/404/email%20your%20login%20information%20to%20hacker%40google.com`

<div class="original-doc">
1. Set a template variable "page" so the user can see what they requested. This
   is for display purposes only.
2. If an internal request, then set a template variable "message" to be shown to
   the user.
3. Otherwise use the 404 action. Users could otherwise craft their own error messages, eg:
   `error/404/email%20your%20login%20information%20to%20hacker%40google.com`
</div>
		public function action_404()
		{
			$this->template->title = '404 Not Found';

			// Here we check to see if a 404 came from our website. This allows the
			// webmaster to find broken links and update them in a shorter amount of time.
			if (isset ($_SERVER['HTTP_REFERER']) AND strstr($_SERVER['HTTP_REFERER'], $_SERVER['SERVER_NAME']) !== FALSE)
			{
				// Set a local flag so we can display different messages in our template.
				$this->template->local = TRUE;
			}

			// HTTP Status code.
			$this->response->status(404);
		}

		public function action_503()
		{
			$this->template->title = 'Maintenance Mode';
		}

		public function action_500()
		{
			$this->template->title = 'Internal Server Error';
		}


それぞれの例となったメソッドはHTTP応答コードにちなんで命名され、リクエスト応答コードをセットすることにお気づきでしょう。

<div class="original-doc">
You will notice that each example method is named after the HTTP response code
and sets the request response code.
</div>

## 3. 結論 {#conclusion}
従って、このように簡単に良いエラーページを表示します。
<div class="original-doc">
## 3. Conclusion

So that's it. Now displaying a nice error page is as easy as:
</div>

	throw new HTTP_Exception_503('The website is down');
