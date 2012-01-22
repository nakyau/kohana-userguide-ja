# クラスの読み込み {#loading-classes}


KohanaはPHPの[オートロード](http://php.net/manual/language.oop5.autoload.php)を上手く利用しています。 これはクラスを使用する前に[include](http://php.net/include) や [require](http://php.net/require) を呼び出す必要を取り除きます。 
あなたがクラスを使うときKohanaはクラスファイルを捜し、インクルードます。
例えば [Cookie::set] メソッドを使いたいときは、シンプルにこう呼び出します。

    Cookie::set('mycookie', 'any string value');

あるいは [Encrypt]インスタンスを読み込むには、[Encrypt::instance]を呼び出します。

<div class="original-doc">
# Loading Classes

Kohana takes advantage of PHP [autoloading](http://php.net/manual/language.oop5.autoload.php). This removes the need to call [include](http://php.net/include) or [require](http://php.net/require) before using a class. When you use a class Kohana will find and include the class file for you. For instance, when you want to use the [Cookie::set] method, you simply call:

    Cookie::set('mycookie', 'any string value');

Or to load an [Encrypt] instance, just call [Encrypt::instance]:
</div>

    $encrypt = Encrypt::instance();

クラスは[Kohana::auto_load]メソッドによってロードされます。クラス名からファイル名に単純に変換されます。

1. クラスは [ファイルシステム](files)の`classes/`ディレクトリに配置されます。
2. アンダースコア文字は全てスラッシュに変換されます。
3. ファイル名は小文字です。

ロードされていないクラスを呼び出したとき、 (例: `Session_Cookie`), Kohana は [Kohana::find_file]を使って`classes/session/cookie.php`という名前のファイルをファイルシステムから検索します。

もしあなたのクラスがこのルールに従っていなければ、それらはKohanaによってオートロードされることはできません。あなたは自らの手でそのファイルをインクルードするか、あるいはあなた独自の[オートロード機能](http://us3.php.net/manual/en/function.spl-autoload-register.php)を加える必要があるでしょう。

<div class="original-doc">
Classes are loaded via the [Kohana::auto_load] method, which makes a simple conversion from class name to file name:

1. Classes are placed in the `classes/` directory of the [filesystem](files)
2. Any underscore characters in the class name are converted to slashes
2. The filename is lowercase

When calling a class that has not been loaded (eg: `Session_Cookie`), Kohana will search the filesystem using [Kohana::find_file] for a file named `classes/session/cookie.php`.

If your classes do not follow this convention, they cannot be autoloaded by Kohana.  You will have to manually included your files, or add your own [autoload function.](http://us3.php.net/manual/en/function.spl-autoload-register.php)
</div>

## カスタムオードローダー {#custom-autoloaders}

Kohanaのデフォルトのオートローダーは`application/bootstrap.php`で[spl_autoload_register](http://php.net/spl_autoload_register)を使って有効にされます。

    spl_autoload_register(array('Kohana', 'auto_load'));

これは、クラスが最初に使用される場合まだ存在しないあらゆるクラスを見つけてインクルードすることを[Kohana::auto_load]が試みることを可能にします。

<div class="original-doc">
## Custom Autoloaders

Kohana's default autoloader is enabled in `application/bootstrap.php` using [spl_autoload_register](http://php.net/spl_autoload_register):

    spl_autoload_register(array('Kohana', 'auto_load'));

This allows [Kohana::auto_load] to attempt to find and include any class that does not yet exist when the class is first used.
</div>

### 例: Zend {#exsample-zend}

ライブラリにオートローダーが組み込まれている場合は簡単にそのライブラリにアクセスすることができます。
例えばこれはZendのオートローダーを有効にする方法です。これでZendのライブラリをKohanaのアプリケーションで使用できます。

<div class="original-doc">
### Example: Zend

You can easily gain access to other libraries if they include an autoloader.  For example, here is how to enable Zend's autoloader so you can use Zend libraries in your Kohana application.
</div>

#### Zend Frameworkファイルのダウンロードとインストール {#download-and-install-the-zend-framework-files}

- [最新のZend Frameworkをダウンロードする](http://framework.zend.com/download/latest).
- `application`ディレクトリに`vendor`ディレクトリを作成する。これはサードパーティのアプリケーションをあなたのアプリケーションのクラスファイルと分離します。
- Zend Frameworkを含む、解凍したZendフォルダを`application/vendor/Zend`に移動する。

<div class="original-doc">
#### Download and install the Zend Framework files

- [Download the latest Zend Framework files](http://framework.zend.com/download/latest).
- Create a `vendor` directory at `application/vendor`. This keeps third party software separate from your application classes.
- Move the decompressed Zend folder containing Zend Framework to `application/vendor/Zend`.
</div>

#### bootstrapにZendのオートローダーをインクルードする

`application/bootstrap.php`のどこかに以下のコードをコピーします。:

<div class="original-doc">
#### Include Zend's Autoloader in your bootstrap

Somewhere in `application/bootstrap.php`, copy the following code:
</div>

	/**
	 * Enable Zend Framework autoloading
	 */
	if ($path = Kohana::find_file('vendor', 'Zend/Loader'))
	{
	    ini_set('include_path',
	    ini_get('include_path').PATH_SEPARATOR.dirname(dirname($path)));
	
	    require_once 'Zend/Loader/Autoloader.php';
	    Zend_Loader_Autoloader::getInstance();
	}

#### 使用例 {#usage-exsample}

これであらゆるZend FrameworkクラスをKohanaのアプリケーションからオートロードできるようになります。

<div class="original-doc">	
#### Usage example

You can now autoload any Zend Framework classes from inside your Kohana application.
</div>

	if ($validate($_POST))
	{
		$mailer = new Zend_Mail;
		
		$mailer->setBodyHtml($view)
			->setFrom(Kohana::$config->load('site')->email_from)
			->addTo($email)
			->setSubject($message)
			->send();
	}