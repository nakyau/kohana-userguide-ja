# ブートストラップ {#bootstrap}

ブートストラップはapplication/bootstrap.phpに配置されています。Kohanaの環境をセットアップすることとメインのレスポンスを実行する起点となります。ブートストラップはindex.phpからインクルードされます。([Request flow](flow)を見てください。)

[!!] ブートストラップはアプリケーションのフローの起点になります。以前のバージョンのKohanaではブートストラップは`system`にあり、多少の見えない部分、変更できない部分がありました。Kohana3ではブートストラップはより不可欠で広い役割を引き受けます。あなたが適切と思うようにブートストラップを編集することをためらわないでください。

<div class="original-doc">
# Bootstrap

The bootstrap is located at `application/bootstrap.php`.  It is responsible for setting up the Kohana environment and executing the main response. It is included by `index.php` (see [Request flow](flow))

[!!] The bootstrap is responsible for the flow of your application.  In previous versions of Kohana the bootstrap was in `system` and was somewhat of an unseen, uneditible force.  In Kohana 3 the bootstrap takes on a much more integral and versatile role.  Do not be afraid to edit and change your bootstrap however you see fit.
</div>
## 環境設定 {#environment-setup}

はじめにブートストラップはタイムゾーンとロケールを設定します。そして[カスケーディングファイルシステム](files)が動作する様にKohanaのオートローダーを追加します。
アプリケーションに必要な設定は全てここに追加できます。


<div class="original-doc">
## Environment setup

First the bootstrap sets the timezone and the locale, and adds Kohana's autoloader so the [cascading filesystem](files) works.  You could add any other settings that all your application needed here.
</div>

~~~
// 簡単なコメントが付いたbootstrap.php からのサンプル抜粋

// デフォルトのタイムゾーンを設定
date_default_timezone_set('America/Chicago');
 
// デフォルトのロケールを設定
setlocale(LC_ALL, 'en_US.utf-8');
 
// Kohanaのオートローダーを有効化
spl_autoload_register(array('Kohana', 'auto_load'));
 
// シリアライズされていない関数用の Kohana オートローダーを有効化
ini_set('unserialize_callback_func', 'spl_autoload_call');
~~~
<div class="original-doc">
~~~
// Sample excerpt from bootstrap.php with comments trimmed down

// Set the default time zone.
date_default_timezone_set('America/Chicago');
 
// Set the default locale.
setlocale(LC_ALL, 'en_US.utf-8');
 
// Enable the Kohana auto-loader.
spl_autoload_register(array('Kohana', 'auto_load'));
 
// Enable the Kohana auto-loader for unserialization.
ini_set('unserialize_callback_func', 'spl_autoload_call');
~~~
</div>

## 初期化と設定 {#initialization-and-configration}

Kohanaは[Kohana::init]が呼ばれるときに初期化されます。そしてログと[設定ファイル](files/config)のリーダーとライターが有効になります。

<div class="original-doc">
## Initialization and Configuration

Kohana is then initialized by calling [Kohana::init], and the log and [config](files/config) reader/writers are enabled. 
</div>

~~~
// 簡単なコメントが付いたbootstrap.php からのサンプル抜粋

// デフォルトのタイムゾーンをセット
date_default_timezone_set('America/Chicago');

// ログにファイルライターをアタッチ。 複数のライターがサポートされています。
Kohana::$log->attach(new Kohana_Log_File(APPPATH.'logs'));

// 設定にファイルリーダーをアタッチ。複数のリーダーがサポートされています。
Kohana::$config->attach(new Kohana_Config_File);
~~~
<div class="original-doc">
~~~
// Sample excerpt from bootstrap.php with comments trimmed down

Kohana::init(array('
    base_url' => '/kohana/',
	index_file => false,
));

// Attach the file writer to logging. Multiple writers are supported.
Kohana::$log->attach(new Kohana_Log_File(APPPATH.'logs'));

// Attach a file reader to config. Multiple readers are supported.
Kohana::$config->attach(new Kohana_Config_File);
~~~
</div>

ブートストラップにある設定に基づいて異なる値を持たせるために、条件文を追加することができます。例えば、`$_SERVER['HTTP_HOST']`をチェックすることで本番サーバであるかを検知して、キャッシュやプロファイルなどをセットします。これは単に例であり、同じ事をおこなうのに様々な方法があります。


<div class="original-doc">
You can add conditional statements to make the bootstrap have different values based on certain settings.  For example, detect whether we are live by checking `$_SERVER['HTTP_HOST']` and set caching, profiling, etc. accordingly.  This is just an example, there are many different ways to accomplish the same thing.
</div>
~~~
	// http://github.com/isaiahdw/kohanaphp.com/blob/f2afe8e28b/application/bootstrap.phpからの抜粋
	... [省略]
	
	/**
	* Set the environment status by the domain.
	 */
	if (strpos($_SERVER['HTTP_HOST'], 'kohanaphp.com') !== FALSE)
	{
	 	// We are live!
	  	Kohana::$environment = Kohana::PRODUCTION;
	     
	  // Turn off notices and strict errors
	  error_reporting(E_ALL ^ E_NOTICE ^ E_STRICT);
	}
	 
	/**
	 * Initialize Kohana, setting the default options.
	 ... [省略]
	 */
	Kohana::init(array(
		'base_url'   => Kohana::$environment === Kohana::PRODUCTION ? '/' : '/kohanaphp.com/',
		'caching'    => Kohana::$environment === Kohana::PRODUCTION,
		'profile'    => Kohana::$environment !== Kohana::PRODUCTION,
		'index_file' => FALSE,
	));
	
	... [省略]

~~~


<div class="original-doc">

~~~
// Excerpt from http://github.com/isaiahdw/kohanaphp.com/blob/f2afe8e28b/application/bootstrap.php
... [trimmed]
 
/**
 * Set the environment status by the domain.
 */
if (strpos($_SERVER['HTTP_HOST'], 'kohanaphp.com') !== FALSE)
{
	// We are live!
	Kohana::$environment = Kohana::PRODUCTION;
 
	// Turn off notices and strict errors
	error_reporting(E_ALL ^ E_NOTICE ^ E_STRICT);
}
 
/**
 * Initialize Kohana, setting the default options.
 ... [trimmed]
 */
Kohana::init(array(
	'base_url'   => Kohana::$environment === Kohana::PRODUCTION ? '/' : '/kohanaphp.com/',
	'caching'    => Kohana::$environment === Kohana::PRODUCTION,
	'profile'    => Kohana::$environment !== Kohana::PRODUCTION,
	'index_file' => FALSE,
));

... [trimmed]

~~~
</div>

[!!]注意：デフォルトのブートストラップは`Kohana::$environment = $env['KOHANA_ENV']`をセットします。このお変数を与える方法のDocsはあなたのウェブサーバーのドキュメントにあります。（例えばApache(http://httpd.apache.org/docs/1.3/mod/mod_env.html#setenv),Lighttpd(http://redmine.lighttpd.net/wiki/1/Docs:ModSetEnv#Options)）。設定オプションやホストネームにかかわらず、サーバーごとに設定を変更できるので、これは他の方法で`Kohana::$environment`を設定するより良い方法であると考えます。

<div class="original-doc">
[!!] Note: The default bootstrap will set `Kohana::$environment = $_ENV['KOHANA_ENV']` if set. Docs on how to supply this variable are available in your web server's documentation (e.g. [Apache](http://httpd.apache.org/docs/1.3/mod/mod_env.html#setenv), [Lighttpd](http://redmine.lighttpd.net/wiki/1/Docs:ModSetEnv#Options)). This is considered better practice than many alternative methods to set `Kohana::$enviroment`, as you can change the setting per server, without having to rely on config options or hostnames.
</div>

## モジュール {#modules}

**詳細な説明は[モジュール](modules)を読んでください。**

[モジュール](modules) は[Kohana::modules()]を使ってロードされます。
含まれているモジュールはオプションです。 

各モジュール名がキーとして、モジュールへの絶対パスまたは相対パスが値として配列に入っています。

<div class="original-doc">
## Modules

**Read the [Modules](modules) page for a more detailed description.**

[Modules](modules) are then loaded using [Kohana::modules()].  Including modules is optional.  

Each key in the array should be the name of the module, and the value is the path to the module, either relative or absolute.
</div>

~~~
// bootstrap.php からの例

Kohana::modules(array(
	'database'   => MODPATH.'database',
	'orm'        => MODPATH.'orm',
	'userguide'  => MODPATH.'userguide',
));
~~~
<div class="original-doc">
~~~
// Example excerpt from bootstrap.php

Kohana::modules(array(
	'database'   => MODPATH.'database',
	'orm'        => MODPATH.'orm',
	'userguide'  => MODPATH.'userguide',
));
~~~
</div>
## ルート {#routes}

**詳細な説明とさらなる例については[Routing](routing) を読んでください。**
[Routes](routing) は [Route::set()]で定義されます。  
<div class="original-doc">
## Routes

**Read the [Routing](routing) page for a more detailed description and more examples.**

[Routes](routing) are then defined via [Route::set()].  
</div>

~~~
// Kohana 3 からのデフォルトルート
Route::set('default', '(<controller>(/<action>(/<id>)))')
	->defaults(array(
		'controller' => 'welcome',
		'action'     => 'index',
	));
~~~
<div class="original-doc">

~~~
// The default route that comes with Kohana 3
Route::set('default', '(<controller>(/<action>(/<id>)))')
	->defaults(array(
		'controller' => 'welcome',
		'action'     => 'index',
	));
~~~
</div>