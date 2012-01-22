# 設定ファイル {#config-files}

設定ファイルはモジュールやクラスが必用とする設定を登録するために使用されます。設定ファイルは`config`ディレクトリに置かれた通常のphpファイルで連想配列を返します:

<div class="original-doc">
# Config Files

Configuration files are used to store any kind of configuration needed for a module, class, or anything else you want.  They are plain PHP files, stored in the `config/` directory, which return an associative array:
</div>


    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'setting' => 'value',
        'options' => array(
            'foo' => 'bar',
        ),
    );

上記の設定ファイルが`myconf.php`という名前なら、以下のようにしてこの設定にアクセスできます。
<div class="original-doc">
If the above configuration file was called `myconf.php`, you could access it using:
</div>


    $config = Kohana::$config->load('myconf');
    $options = $config->get('options')

## マージ {#merge}

設定ファイルは[カスケーディングファイルシステム](files)によって上書きではなく**マージ**されるという点でほかの多くのファイルと違います。
これは最終の設定として全ての設定ファイルが組み合わせられることを意味します。全てのファイルの重複させることなく*個々の設定*をオーバーロードすることができます。

例えば、ある設定ファイルに何か変更を加えたいならば、元の設定ファイルにある重複したほかのエントリーは必要ありません。

    // config/inflector.php

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'irregular' => array(
            'die' => 'dice', // デフォルトの設定ファイルには存在しない
            'mouse' => 'mouses', // デフォルトの設定ファイルの 'mouse' => 'mice' を上書きする
    );

<div class="original-doc">
## Merge

Configuration files are slightly different from most other files within the [cascading filesystem](files) in that they are **merged** rather than overloaded. This means that all configuration files with the same file path are combined to produce the final configuration. The end result is that you can overload *individual* settings rather than duplicating an entire file.

For example, if we wanted to change or add to an entry in the inflector configuration file, we would not need to duplicate all the other entries from the default configuration file.

    // config/inflector.php

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'irregular' => array(
            'die' => 'dice', // does not exist in default config file
            'mouse' => 'mouses', // overrides 'mouse' => 'mice' in the default config file
    );
</div>

ウェブサイトのタイトルやGoogle Analyticsのコードなどを保存し簡単に変更できる設定ファイルが必要であるとしましょう。
設定ファイルを作ります。これをsite.phpと呼びましょう。


    // config/site.php

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'title' => 'Our Shiny Website',
        'analytics' => FALSE, // analytics のコードをここに書く,、無効にするときはFALSE
    );

これで`Kohana::config('site.title')`でサイト名を呼び出せます。Google Analyticsのコードは`Kohana::config('site.analytics')`で呼び出せます。

<div class="original-doc">
## Creating your own config files

Let's say we want a config file to store and easily change things like the title of a website, or the google analytics code.  We would create a config file, let's call it `site.php`:


    // config/site.php

    <?php defined('SYSPATH') or die('No direct script access.');

    return array(
        'title' => 'Our Shiny Website',
        'analytics' => FALSE, // analytics code goes here, set to FALSE to disable
    );

We could now call `Kohana::$config->load('site.title')` to get the site name, and `Kohana::$config->load('site.analytics')` to get the analytics code.
</div>

あるソフトウェアのバージョンをアーカイブをしたいとしましょう。設定ファイルを使って各バージョンを格納し、且つダウンロードのリンク、ドキュメント、課題トラッキングの値を格納できます。
<div class="original-doc">
Let's say we want an archive of versions of some software.  We could use config files to store each version, and include links to download, documentation, and issue tracking.
</div>


	// config/versions.php

	<?php defined('SYSPATH') or die('No direct script access.');
	
    return array(
		'1.0.0' => array(
			'codename' => 'Frog',
			'download' => 'files/ourapp-1.0.0.tar.gz',
			'documentation' => 'docs/1.0.0',
			'released' => '06/05/2009',
			'issues' => 'link/to/bug/tracker',
		),
		'1.1.0' => array(
			'codename' => 'Lizard',
			'download' => 'files/ourapp-1.1.0.tar.gz',
			'documentation' => 'docs/1.1.0',
			'released' => '10/15/2009',
			'issues' => 'link/to/bug/tracker',
		),
		/// ... etc ...
	);

それから以下のようにできます:

	// コントローラーの記述
	$view->versions = Kohana::$config->load('versions');
	
	// ビューの記述
	foreach ($versions as $version)
	{
		// バージョンごとに何かHTMLを表示する
	}

<div class="original-doc">
You could then do the following:

	// In your controller
	$view->versions = Kohana::$config->load('versions');
	
	// In your view:
	foreach ($versions as $version)
	{
		// echo some html to display each version
	}
</div>

