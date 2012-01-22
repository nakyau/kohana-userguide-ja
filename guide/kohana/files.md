# カスケーディングファイルシステム {#cascading-filesystem}

Kohanaのファイルシステムはディレクトリ構造ににたカスケードになっています。
Kohanaの階層([Kohana::find_file]によってファイルを読み込む場合)は以下の順番になっています。

<div class="original-doc">
# Cascading Filesystem

The Kohana filesystem is a hierarchy of similar directory structures that cascade. The hierarchy in Kohana (used when a file is loaded by [Kohana::find_file]) is in the following order:
</div>

1. **アプリケーションパス**  
   `APPPATH`として`index.php`に定義されます。初期値は`application`です。

2. **モジュールパス**  
   `APPPATH/bootstrap.php`の中で[Kohana::modules]を使用して、連想配列としてセットされます。 配列の個々の値は**モジュールが定義された順番**に探索されるでしょう。

3. **システムパス**  
   `SYSTEMPATH`として`index.php`に定義されています。初期値は`system`です。すべての主要なまたはコアのファイルとクラスはここで定義されます。

インクルード・パスより上位にあるディレクトリの中のファイルは、より低い階層にある同じ名前のファイルより優先されます。これは"より高い"階層にある同じ名前のファイルのオーバーロードを可能にするためです。

<div class="original-doc">
1. **Application Path**  
   Defined as `APPPATH` in `index.php`. The default value is `application`.

2. **Module Paths**  
   This is set as an associative array using [Kohana::modules] in `APPPATH/bootstrap.php`. Each of the values of the array will be searched **in the order that the modules are defined**.

3. **System Path**  
   Defined as `SYSPATH` in `index.php`. The default value is `system`. All of the main or "core" files and classes are defined here.
</div>

<div class="original-doc">
Files that are in directories higher up the include path order take precedence over files of the same name lower down the order, which makes it is possible to overload any file by placing a file with the same name in a "higher" directory:
</div>

![Cascading Filesystem Infographic](cascading_filesystem.png)

このイメージは単にファイルの場所を示すものですが、これからカスケーディンク・ファイルシステムのいくつかの例を説明するのに使います:

* kohanaがエラーをキャッチすれば、`kohana/error.php`を表示します。そしてそれは`Kohana::find_file('views','kohana/error')`を呼び出すでしょう。その結果としては`application/views/kohana/error.php`を返します。なぜならば、これは`system/views/kohana/error.php`より優先されるからです。

* `View::factory('welcome')`を使用して、`Kohana::find_file('views','welcome')`を呼び出せば`application/views/welcome.php`を返すでしょう。なぜならば、それは`modules/common/views/welcome.php`より優先されるからです。この仕組みを利用してモジュールファイルを編集しないで処理を上書きできます。

* Cookieクラスを使用すれば、`Kohana::auto_loadはKohana::find_file('classes','cookie')`を呼び出し、それは`application/classes/cookie.php`を返すでしょう。CookieがKohana_Cookieを拡張すると仮定して、オートローダーは`Kohana::find_file('classes','kohana/cookie')`を呼び出し、`system/classes/kohana/cookie.php`を返します。なぜならば、そのファイルはカスケードの高位のどこにも存在しないからです。

* `View::factory('user')`を使用すれば、それは`Kohana::find_file('views','user')`を呼び出し、`modules/common/views/user.php`を返すでしょう。

* `config/database.php`を変更したければ、`application/config/database.php`にファイルをコピーし、コピーしたファイルを変更します。設定ファイルはカスケーディングによって上書きされるのではなく、[マージ](files/config#merge)されることに注意してください。

<div class="original-doc">
This image is only shows certain files, but we can use it to illustrate some examples of the cascading filesystem:

* If Kohana catches an error, it would display the `kohana/error.php` view, So it would call `Kohana::find_file('views', 'kohana/error')`.  This would return `application/views/kohana/error.php` because it takes precidence over `system/views/kohana/error.php`.  By doing this we can change the error view without editing the system folder.

* If we used `View::factory('welcome')` it would call `Kohana::find_file('views','welcome')` which would return `application/views/welcome.php` because it takes precidence over `modules/common/views/welcome.php`.  By doing this, you can overwrite things in a module without editing the modules files.

* If use the Cookie class, [Kohana::auto_load] will call `Kohana::find_file('classes', 'cookie')` which will return `application/classes/cookie.php`.  Assuming Cookie extends Kohana_Cookie, the autoloader would then call `Kohana::find_file('classes','kohana/cookie')` which will return `system/classes/kohana/cookie.php` because that file does not exist anywhere higher in the cascade.  This is an example of [transparent extension](extension).

* If you used `View::factory('user')` it would call `Kohana::find_file('views','user')` which would return `modules/common/views/user.php`.

* If we wanted to change something in `config/database.php` we could copy the file to `application/config/database.php` and make the changes there.  Keep in mind that [config files are merged](files/config#merge) rather than overwritten by the cascade.
</div>

## ファイルの種類 {#types-of-files}

application,modules,systemのトップレベルディレクトリは以下のデフォルトディレクトリを持っています:

classes/
:  [オートロード](autoloading)したいクラスはすべてここに置いてください。これは[controllers](mvc/controllers), [models](mvc/models)そして他のすべてのクラスも含みます。すべてのクラスは[クラス命名規則](conventions#class-names-and-file-location)に従わなければなりません。

config/
:  ファイルは[Kohana::$config]を使って読み込まれるオプションの連想配列を返します。設定ファイルはカスケードによって上書きではなく、マージされます。さらなる情報は[設定ファイル](files/config)を参照してください。


i18n/
:  翻訳ファイルは文字列の連想配列を返します。翻訳は`__()`メソッドを使うことで実行されます。"Hello, world"をスペイン語に翻訳するには、[l18n::$lang]に"es-es"をセットしてから`__('Hello, world')`を呼び出してください。
i18nファイルはカスケードによって上書きではなく、マージされます。さらなる情報は[I18n ファイル](files/i18n)を参照してください。

messages/
:  メッセージファイルは[Kohana::message]を使って読み込まれる文字列の連想配列を返します。メッセージファイルとi18nファイルの違いはメッセージファイルは翻訳されず、いつでもデフォルトの言語で書き出され、１つのキーによって参照されるということです。メッセージファイルはカスケードによって上書きではなく、マージされます。さらなる情報は[メッセージファイル](files/messages) を参照してください。

views/
:  ビューはHTMLを生成したり異なる形式の出力を行う通常のphpファイルです。ビューファイルはViewオブジェクトの中でロードされ値を設定されます。そのあとでHTMLの一部に変換されます。複数のビューは互いに参照することができます。さらなる情報は[ビュー](mvc/views)を参照してください。

*その他*
:  カスケーディングファイルシステムにはその他のどんなフォルダをも含めることができます。`guide`,`vendor`,`media`など望みのものを制限なく加えられます。例えば、カスケーディングファイルシステムの`media/logo.png`は`Kohana::find_file('media','logo','png')`で呼び出せます。

<div class="original-doc">
## Types of Files

The top level directories of the application, module, and system paths have the following default directories:

classes/
:  All classes that you want to [autoload](autoloading) should be stored here. This includes [controllers](mvc/controllers), [models](mvc/models), and all other classes. All classes must follow the [class naming conventions](conventions#class-names-and-file-location).

config/
:  Configuration files return an associative array of options that can be loaded using [Kohana::$config]. Config files are merged rather than overwritten by the cascade. See [config files](files/config) for more information.

i18n/
:  Translation files return an associative array of strings. Translation is done using the `__()` method. To translate "Hello, world!" into Spanish, you would call `__('Hello, world!')` with [I18n::$lang] set to "es-es". I18n files are merged rather than overwritten by the cascade. See [I18n files](files/i18n) for more information.

messages/
:  Message files return an associative array of strings that can be loaded using [Kohana::message]. Messages and i18n files differ in that messages are not translated, but always written in the default language and referred to by a single key. Message files are merged rather than overwritten by the cascade. See [message files](files/messages) for more information.

views/
:  Views are plain PHP files which are used to generate HTML or other output. The view file is loaded into a [View] object and assigned variables, which it then converts into an HTML fragment. Multiple views can be used within each other. See [views](mvc/views) for more information.

*other*
:  You can include any other folders in your cascading filesystem.  Examples include, but are not limited to, `guide`, `vendor`, `media`, whatever you want.  For example, to find `media/logo.png` in the cascading filesystem you would call `Kohana::find_file('media','logo','png')`.
</div>

## ファイル検索 {#finding-files}

ファイルシステム上のいかなるファイルでも[Kohana::find_file]で検索できます:

    // フルパス"classes/cookie.php"を検索
    $path = Kohana::find_file('classes', 'cookie');

    // フルパス"views/user/login.php"を検索
    $path = Kohana::find_file('views', 'user/login');
	
拡張子が`.php`ではないファイルを検索するときは第３引数に拡張子を指定します。

	// フルパス"guide/menu.md"を検索
	$path = Kohana::find_file('guide', 'menu', 'md');

	// $name が"2000-01-01-first-post" ならば
	// "posts/2000-01-01-first-post.textile"を検索する
	$path = Kohana::find_file('posts', $name, '.textile');

<div class="original-doc">
## Finding Files

The path to any file within the filesystem can be found by calling [Kohana::find_file]:

    // Find the full path to "classes/cookie.php"
    $path = Kohana::find_file('classes', 'cookie');

    // Find the full path to "views/user/login.php"
    $path = Kohana::find_file('views', 'user/login');
	
If the file doesn't have a `.php` extension, pass the extension as the third param.

	// Find the full path to "guide/menu.md"
	$path = Kohana::find_file('guide', 'menu', 'md');

	// If $name is "2000-01-01-first-post" this would look for "posts/2000-01-01-first-post.textile"
	$path = Kohana::find_file('posts', $name, '.textile');
</div>
## ベンダーエクステンション {#vendor-extension}

Kohanaに特化していない、ベンダーエクステンションを"エクステンション"または"外部ライブラリ"と呼び、applicationやmoduleではなく、vendorファイルに格納します。なぜならばそれらのライブラリはKohanaのファイル命名規則に従っていないので、Kohanaによってオードロードできません。ですからあなたはそれらを自らの手によってインクルードしなくてないけません。ベンダーライブラリについての幾つかの例が[Markdown](http://daringfireball.net/projects/markdown/), [DOMPDF](http://code.google.com/p/dompdf),  [Mustache](http://github.com/bobthecow/mustache.php)と[Swiftmailer](http://swiftmailer.org/)にあります。

例えば、あなたが[DOMPDF](http://code.google.com/p/dompdf)を使いたいならば、ライブラリを`application/vendor/dompdf`にコピーしてDOMPDFをオートロードされたクラスからインクルードします。これを行うのにはコントローラーのbeforeメソッドやモジュールのinit.php、シングルトンクラスのコンストラクタが便利でしょう。

    require Kohana::find_file('vendor', 'dompdf/dompdf/dompdf_config','inc');

これで余計なファイルをロードすることなくDOMPDFが使えるようになります:

    $pdf = new DOMPDF;

[!!] DOMPDFを使ってビューをPDFに変換したいのなら、[PDFView](http://github.com/shadowhand/pdfview) を試してください。
<div class="original-doc">
## Vendor Extensions

We call extensions or external libraries that are not specific to Kohana "vendor" extensions, and they go in the vendor folder, either in application or in a module.  Because these libraries do not follow Kohana's file naming conventions, they cannot be autoloaded by Kohana, so you will have to manually included them. Some examples of vendor libraries are [Markdown](http://daringfireball.net/projects/markdown/), [DOMPDF](http://code.google.com/p/dompdf),  [Mustache](http://github.com/bobthecow/mustache.php) and [Swiftmailer](http://swiftmailer.org/).

For example, if you wanted to use [DOMPDF](http://code.google.com/p/dompdf), you would copy it to `application/vendor/dompdf` and include the DOMPDF autoloading class.  It can be useful to do this in a controller's before method, as part of a module's init.php, or the contstructor of a singleton class.

    require Kohana::find_file('vendor', 'dompdf/dompdf/dompdf_config','inc');

Now you can use DOMPDF without loading any more files:

    $pdf = new DOMPDF;

[!!] If you want to convert views into PDFs using DOMPDF, try the [PDFView](http://github.com/shadowhand/pdfview) module.
</div>
