# 設定 {#configration}

デフォルトのKohanaはカスケーディングダイル上で[設定ファイル](files/config)から設定値を読み込むセットアップになっています。
しかしながら、ほかの場所にある／異なった書式の設定値の読み込みに適用させることは簡単にできます。
<div class="original-doc">
# Configuration

By default kohana is setup to load configuration values from [config files](files/config) in the
cascading filesystem.  However, it is very easy to adapt it to load config values in other 
locations/formats.
</div>

## ソース {#sources}
システムは**Config Sources**のコンセプトに沿って設計されています。設定値の配置にはわずかな意味しかありません。

ソースから設定を読み込むのに**Config Reader**が必要です。同様に設定をソースに書き込むのに**Config Writer**が必要です。

[Kohana_Config_Reader] / [Kohana_Config_Writer] インターフェースを拡張して、これらをごくシンプルに実装します:

<div class="original-doc">
## Sources

The system is designed around the concept of **Config Sources**, which loosely means a method of
storing configuration values.

To read config from a source you need a **Config Reader**. Similarly, to write config to a source
you need a **Config Writer**.

Implementing them is as simpe as extending the 
[Kohana_Config_Reader] / [Kohana_Config_Writer] interfaces:
</div>

	class Kohana_Config_Database_Reader implements Kohana_Config_Reader
	class Kohana_Config_Database_Writer extends Kohana_Config_Database_Reader implements Kohana_Config_Writer

上記の例でデータベースライターがデータベースリーダーを拡張していることに気かつくでしょう。
これはConfig Sourceの規則で、ソースに書き込めば同じように読むこともできます。しかし、この規則は強制ではなく、開発者に一考の余地を残しています。
<div class="original-doc">
You'll notice in the above example that the Database Writer extends the Database Reader. 
This is the convention with config sources, the reasoning being that if you can write to a 
source chances are you can also read from it as well. However, this convention is not enforced 
and is left to the developer's discretion.
</div>

## グループ {#groups}

補助構成のために設定値は論理的なグループに分割されます。
例えばデータベースに関連した設定は`database`グループに、そしてセッションに関連した設定は`session`グループにあります。

これらのグループがどのようにして格納/構成させるかはConfig Sourceで決定されます。
たとえばファイルソースは異なるファイル(`database.php`, `session.php`)に異なる設定グループを入れます。
しかしデータベースソースはグループを識別するためにカラムを使用します。

設定グループを読み込むにはシンプルに`Kohana::$config->load()`にグループ名を指定して呼び出します:
<div class="original-doc">
## Groups

In order to aide organisation config values are split up into logical "groups".  For example
database related settings go in a `database` group, and session related settings go in a 
`session` group.

How these groups are stored/organised is up to the config source.  For example the file source
puts different config groups into different files (`database.php`, `session.php`) whereas
the database source uses a column to distinguish between groups.

To load a config group simply call `Kohana::$config->load()` with the name of the group you wish to load:
</div>

	$config = Kohana::$config->load('my_group');

`load()`は設定値をカプセル化してどんな変更でもコンフィグライターに渡すことを保証する[Config_Group]のインスタンスを返します。

[Config_Group]オブジェクトから設定値を得るには、ただ単に[Config_Group::get]を呼び出します。

<div class="original-doc">
`load()` will return an instance of [Config_Group] which encapsulates the config values and ensures
that any modifications made will be passed back to the config writers.

To get a config value from a [Config_Group] object simply call [Config_Group::get]:
</div>

	$config = Kohana::$config->load('my_group');
	$value  = $config->get('var');

値を変更するには[Config_Group::set]を呼び出します:
<div class="original-doc">
To modify a value call [Config_Group::set]:
</div>

	$config = Kohana::$config->load('my_group');
	$config->set('var', 'new_value');

### 設定を読み書きする別の方法 {#alternativ-methods}

上記に説明したメソッドに加えて、ドット記法で設定グループの目的の値へにパスでアクセスすることもできます:

	// 設定ファイル: database.php
	return array(
		'default' => array(
			'connection' => array(
				'hostname' => 'localhost'
			)
		)
	);
	
	// ホスト名を必要とするコード:
	$hostname = Kohana::$config->load('database.default.connection.hostname');

<div class="original-doc">
### Alternative methods for getting / setting config

In addition to the methods described above you can also access config values using dots to outline a path
from the config group to the value you want:

	// Config file: database.php
	return array(
		'default' => array(
			'connection' => array(
				'hostname' => 'localhost'
			)
		)
	);
	
	// Code which needs hostname:
	$hostname = Kohana::$config->load('database.default.connection.hostname');
</div>

これは以下と等価です:

<div class="original-doc">
Which is equivalent to:
</div>

	$config = Kohana::$config->load('database')->get('default');
	
	$hostname = $config['connection']['hostname'];

明らかにこちらのほうが元のコードよりコンパクトです。
`ドット記法`は沢山の`get()`を呼び出すより遅く、その度に配列を横断しなければならないことを気に止めておいてください。
ドット記法は一つの特定の値を取得するときには便利です。しかし、そうでなけば`get()`を使うのがベストです。
<div class="original-doc">
Obviously this method is a lot more compact than the original, however please bear in mind that using
`dot.notation` is a _lot_ slower than calling `get()` and traversing the array yourself.  Dot notation
can be useful if you only need one specific variable, but otherwise it's best to use `get()`.
</div>

[Config_Group]が[Array_Object](http://php.net/manual/en/class.arrayobject.php)を拡張するように、配列構文で設定値を取得／書込みできます。

	$config = Kohana::$config->load('database');
	
	// 値を取得
	$hostname = $config['default']['connection']['hostname'];
	
	// 値をセット
	$config['default']['connection']['hostname'] = '127.0.0.1';

繰り返しますが、この書き方は`get()` / `set()`よりコストがかかります。

<div class="original-doc">
As [Config_Group] extends [Array_Object](http://php.net/manual/en/class.arrayobject.php) you can also use array
syntax to get/set config vars:

	$config = Kohana::$config->load('database');
	
	// Getting the var
	$hostname = $config['default']['connection']['hostname'];
	
	// Setting the var
	$config['default']['connection']['hostname'] = '127.0.0.1';

Again, this syntax is more costly than calling `get()` / `set()`.
</div>
## 設定のマージ {#config-merging}

設定システムの便利な機能の一つにグループマージがあります。これは、より低いソースからの設定をより高い位置のスタックの下位のとなるようにマージするカスケーディングファイルシステムと似た動作をします。

2つのソースに同じ設定値を含んでいれば上位のソースが下位のソースをオーバーライドします。
しかしながら、上位のソースに特定の設定値が存在せず、下位のソースにはその設定値が存在する場合には下位のソースが使用されます。


スタックにおけるソース配置は、それらがブートストラップによってどのようにロードされたかによって決定されます。
デフォルトでは、ソースはスタックの最上位にロードされます。
<div class="original-doc">
## Config Merging

One of the useful features of the config system is config group merging. This works in a similar way 
to the cascading filesystem, with configuration from lower sources lower down the source stack being 
merged with sources further up the stack.

If two sources contain the same config variables then the one from the source further up the stack will
override the one from the "lower" source.  However, if the source from higher up the stack does not contain
a particular config variable but a source lower down the stack does then the value from the lower source will
be used.

The position of sources in the stack is determined by how they are loaded in your bootstrap.
By default when you load a source it is pushed to the top of a stack:
</div>

    // Stack: <empty>
	Kohana::$config->attach(new Config_File);
	// Stack: Config_File
	Kohana::$config->attach(new Config_Database);
	// Stack: Config_Database, Config_File

上記の例では、データベースで見つかったすべての設定値はファイルシステムの設定値をオーバーライドします。
例えば、上記のセットアップを使用すれば:

	// ファイルシステムにおける設定:
		email:
			sender: 
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
	
	// データベースにおける設定:
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
	
	// Kohana::$config->load('email') が返す設定値
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
			method: smtp


<div class="original-doc">
In the example above, any config values found in the database will override those found in the filesystem.
For example, using the setup outlined above:

	// Configuration in the filesystem:
		email:
			sender: 
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
	
	// Configuration in the database:
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
	
	// Configuration returned by Kohana::$config->load('email')
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
			method: smtp
</div>

[!!] **注意:** 上記のシンタックスは単に設定をマージする概念を説明するためのコードです。

いくつかの場合において、あなたはスタックの最下位に設定を追加したいと考えるかもしれません。
そうするには、`attach()`の第2引数に`FALSE`を渡します:

<div class="original-doc">
[!!] **Note:** The above syntax is simply pseudo code to illustrate the concept of config merging.

On some occasions you may want to append a config source to the bottom of the stack, to do this pass `FALSE`
as the second parameter to `attach()`:
</div>

	// Stack: <empty>
	Kohana::$config->attach(new Config_File);
	// Stack: Config_File
	Kohana::$config->attach(new Config_Database, FALSE);
	// Stack: Config_File, Config_Database

この例では、ファイルシステムで見つかる全ての値はDBで見つかる値をオーバーライドします。例えば:

	// ファイルシステムにおける設定:
		email:
			sender: 
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
	
	// データベースにおける設定:
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
	
	// Kohana::$config->load('email') が返す設定値
		email:
			sender:
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp

<div class="original-doc">
In this example, any values found in the filesystem will override those found in the db. For example:

	// Configuration in the filesystem:
		email:
			sender: 
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
	
	// Configuration in the database:
		email:
			sender:
				email: my.supercool.address@gmail.com
				name:  Kohana Bot
	
	// Configuration returned by Kohana::$config->load('email')
		email:
			sender:
				email: my.awesome.address@example.com
				name:  Unknown
			method: smtp
</div>
## 環境を元にした異なる設定ソースの使用方法 {#using-different-config-source-based-on-environment}

いくつかの状況において、`Kohana::$environment`に応じた異なる設定値を使う必要がある場合があるでしょう。
ユニットテストがそのような状況の主な例です。（開発からテストを分離するために）多くのセットアップは2つのデータベースを持っています。1つは通常の開発用、もう一つはユニットテスト用です。

この場合はまだ、一般の設定としてあなたのアプリケーションで必要とされる`config`ディレクトリに登録された設定（例えば、暗号化設定）にアクセスする必要があります。
デフォルトの`Config_File`ソースは実際にはオプションではありません。

これらを得るために`config`のサブディレクトリから個別の設定をロードする設定ファイルリーダーをアタッチできます。
<div class="original-doc">
## Using different config sources based on the environment

In some situation's you'll need to use different config values depending on which state `Kohana::$environment`
is in. Unit testing is a prime example of such a situation. Most setups have two databases; one for normal
development and a separate one for unit testing (to isolate the tests from your development).

In this case you still need access to the config settings stored in the `config` directory as it contains generic
settings that are needed whatever environment your application is in (e.g. encryption settings),
)o replacing the default `Config_File` source isn't really an option.

To get around this you can attach a separate config file reader which loads its config from a subdir of `config` called
"testing":
</div>

	Kohana::$config->attach(new Config_File);
	
	Kohana::$config->attach(new Config_Database);
	
	if (Kohana::$environment === Kohana::TESTING)
	{
		Kohana::$config->attach(new Config_File('config/testing'));
	}

普段の開発中は設定ファイルのスタックは`Congif_Database,Config_File('config')`となっています。しかし、`Kohana::$environment === Kohana::TESTING`のときにはスタックは`Config_File('config/testing'), Config_Database, Config_File('config')`となります。
<div class="original-doc">
During normal development the config source stack looks like `Config_Database, Config_File('config')`.  However,
when `Kohana::$environment === Kohana::TESTING` the stack looks like `Config_File('config/testing'), Config_Database, Config_File('config')`
</div>