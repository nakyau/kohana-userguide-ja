# モジュール {#modules}

モジュールとは単に[カスケーディングファイルシステム](files)に追加されるものです。 
モジュールはKohanaで有効な（[Kohana::find_file]経由でアクセスできる）ファイルシステムにいかなる種類のファイル（コントローラー、ビュー、クラス、設定ファイルなど）も追加できます。 
これは異なるアプリケーション間でモジュールを移動したり共有するのに便利です。例えば、モデリングシステム、サーチエンジン、CSSとJavascriptの管理などです。

<div class="original-doc">
# Modules

Modules are simply an addition to the [Cascading Filesystem](files).  A module can add any kind of file (controllers, views, classes, config files, etc.) to the filesystem available to Kohana (via [Kohana::find_file]).  This is useful to make any part of your application more transportable or shareable between different apps.  For example, creating a new modeling system, a search engine, a css/js manager, etc.
</div>

## モジュールを探す {#where-to-find-module}

Kolanos は[kohana-universe](http://github.com/kolanos/kohana-universe/tree/master/modules/)（Githubにある適正に利用可能なモジュールのリスト）を作りました。あなたのモジュールをこのリストに加えるにはGitHub経由で彼にメッセージを送って下さい。

Mon Geslani はGitHubのモジュールをアクティビティ、ウォッチャー、フォークなどでソートする[すばらしいサイト](http://kohana.mongeslani.com/)を作りました。これはkohana-universeのように包括的ではないようです。 

Andrew Hutchings は上記のサイトに似た [kohana-modules](http://www.kohana-modules.com) を作りました。

<div class="original-doc">
## Where to find modules

Kolanos has created [kohana-universe](http://github.com/kolanos/kohana-universe/tree/master/modules/), a fairly comprehensive list of modules that are available on Github. To get your module listed there, send him a message via Github.

Mon Geslani created a [very nice site](http://kohana.mongeslani.com/) that allows you to sort Github modules by activity, watchers, forks, etc.  It seems to not be as comprehensive as kohana-universe.

Andrew Hutchings has created [kohana-modules](http://www.kohana-modules.com) which is similar to the above sites.
</div>

## モジュールの有効化 {#enabling-modules}

モジュールは[Kohana::modules]を呼びだし、'モジュール名' => 'モジュールパス'の配列を与えることで有効になります。モジュール名は重要ではありません。しかし、パスは明らかに重要です。モジュールパスはMODPATHになければならないということはありませんが、通常はMODPATHにあります。[Kohana::modules]は１度だけ呼び出せます。 

<div class="original-doc">
## Enabling modules

Modules are enabled by calling [Kohana::modules] and passing an array of `'name' => 'path'`.  The name isn't important, but the path obviously is.  A module's path does not have to be in `MODPATH`, but usually is.  You can only call [Kohana::modules] once.
</div>

	Kohana::modules(array(
		'auth'       => MODPATH.'auth',       // Basic authentication
		'cache'      => MODPATH.'cache',      // Caching with multiple backends
		'codebench'  => MODPATH.'codebench',  // Benchmarking tool
		'database'   => MODPATH.'database',   // Database access
		'image'      => MODPATH.'image',      // Image manipulation
		'orm'        => MODPATH.'orm',        // Object Relationship Mapping
		'oauth'      => MODPATH.'oauth',      // OAuth authentication
		'pagination' => MODPATH.'pagination', // Paging of results
		'unittest'   => MODPATH.'unittest',   // Unit testing
		'userguide'  => MODPATH.'userguide',  // User guide and API documentation
		));


## Init.php

モジュールが有効化されたとき、そのモジュールのディレクトリに[init.php]があればインクルードされます。init.phpはモジュールが機能するために必要なモジュールのインクルードや初期化を行うのに理想的な場所です。UserguideモジュールとCodeBentchモジュールにはinit.phpがありますので、それを見ることができます。

<div class="original-doc">
When a module is activated, if an `init.php` file exists in that module's directory, it is included.  This is the ideal place to have a module include routes or other initialization necessary for the module to function.  The Userguide and Codebench modules have init.php files you can look at.
</div>

## モジュールの動作 {#how-modules-work}

有効化されたモジュールは仮想的にアプリケーションフォルダの同じ場所に存在するのとほぼ同じようになります。主な違いは[カスケーディングファイルシステム](files)によってより高い位置（これより後に有効にされたモジュール、またはアプリケーションフォルダ）にある同じ名前のファイルによって上書きできるようになることです。

<div class="original-doc">
## How modules work

A file in an enabled module is virtually the same as having that exact file in the same place in the application folder.  The main difference being that it can be overwritten by a file of the same name in a higher location (a module enabled after it, or the application folder) via the [Cascading Filesystem](files).  It also provides an easy way to organize and share your code.
</div>

## 独自のモジュールを作る {#creating-your-own-modules}

モジュールを作成するには単にモジュールフォルダの好きな場所（通常は`DOCROOT/modules`）にフォルダを作成します。そしてブートストラップでモジュールを有効化します。
あなたのモジュールを共有するには[Github](http://github.com)にアップロードしてください。[Kohana](http://github.com/kohana)や[その他のユーザ](#where-to-find-modules)によって作られたモジュールを例として見ることができます。
<div class="original-doc">
## Creating your own module

To create a module simply create a folder (usually in `DOCROOT/modules`) and place the files you want to be in the module there, and activate that module in your bootstrap.  To share your module, you can upload it to [Github](http://github.com).  You can look at examples of modules made by [Kohana](http://github.com/kohana) or [other users](#where-to-find-modules).
</div>
