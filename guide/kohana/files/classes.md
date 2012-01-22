# クラス {#classes}

TODO: Brief intro to classes.

[モデル](mvc/models) と [コントローラー](mvc/controllers) は同様にクラスです。、Kohanaによって少々違う扱いを受けます。もっと学ぶためにはそれぞれのページを読んでください。

<div class="original-doc">
# Classes

TODO: Brief intro to classes.

[Models](mvc/models) and [Controllers](mvc/controllers) are classes as well, but are treated slightly differently by Kohana.  Read their respective pages to learn more.

</div>

## ヘルパーかライブラリか？ {#helper-or-library}

kohana3は旧バージョンのようにヘルパーとライブラリを区別しません。それらは全てclassesフォルダに置かれ、同じ規則に従います。その区別は一般に静的に使用されるのがヘルパー（例えば[Kohanaに含まれるヘルパー](helpers)）、そしてライブラリはインスタンス化されて使われる典型的なオブジェクトです（例えば[データベースクエリ・ビルダー](../database/query/builder)）。その区別ははっきりしてはいません。そしてそのどちらもKohana同じようにでは同じように扱われるので、どちらにしても関係はありません。

<div class="original-doc">
## Helper or Library?

Kohana 3 does not differentiate between "helper" classes and "library" classes like in previous versions.  They are all placed in the `classes/` folder and follow the same conventions.  The distinction is that in general, a "helper" class is used statically,  (for examples see the [helpers included in Kohana](helpers)), and library classes are typically instantiated and used as objects (like the [Database query builders](../database/query/builder)).  The distinction is not black and white, and is irrelevant anyways, since they are treated the same by Kohana.
</div>
## クラスの作成 {#creating-a-class}

新しいクラスを作るには、単に[カスケーディングファイルシステム](files)上のclassesディレクトリに[クラス命名規則](conventions#class-names-and-file-l

<div class="original-doc">
## Creating a class

To create a new class, simply place a file in the `classes/` directory at any point in the [Cascading Filesystem](files), that follows the [Class naming conventions](conventions#class-names-and-file-location).  For example, lets create a `Foobar` class.
</div>

	// classes/foobar.php
	
	class Foobar {
		static function magic() {
			// Does something
		}
	}

これでKohanaはこのファイルを[オートロード](autoloading)するようになるので、`Foobar::magic()`をどこからでも呼び出せます。

サブディレクトリにもクラスを作れます。

<div class="original-doc">	
We can now call `Foobar::magic()` any where and Kohana will [autoload](autoloading) the file for us.

We can also put classes in subdirectories.
</div>
	// classes/professor/baxter.php
	
	class Professor_Baxter {
		static function teach() {
			// Does something
		}
	}
	
これで`Professor_Baxter::teach()`をどこからでも呼び出せます。
クラスを作り使用する例については`system`の`class`フォルダかモジュールを見てください。

<div class="original-doc">
We could now call `Professor_Baxter::teach()` any where we want.

For examples of how to create and use classes, simply look at the 'classes' folder in `system` or any module.
</div>
## Namespacing your classes

TODO: Discuss namespacing to provide transparent extension functionality in your own classes/modules.
