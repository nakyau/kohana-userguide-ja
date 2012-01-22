# 透過的なクラス拡張 {#transparent-class-extension}

[カスケーディングファイルシステム](files) は透過的なファイル拡張を可能にします。 
例えば[Cookie]クラスは`SYSPATH/classes/cookie.php` にこのように定義されています:

    class Cookie extends Kohana_Cookie {}

デフォルトのKohanaクラスや多くのエクステンションはほとんど全てのクラスが拡張できるようにこの定義を使用します。
独自のメソッドを追加するために独自のクラスを`APPPATH/classes/cookie.php`に定義することによって、どんなクラスも透過的に拡張できます。

[!!]  Kohanaとともに配布されたファイルは**決して**変更するべきではありません。アップグレードで起こる問題を防ぐために、どんなときでも透過的拡張を使用してクラスに変更を加えてください。

例えば、 [Encrypt]クラスを使って暗号化したクッキーをセットするクラスを作成したいのであれば、`application/classes/cookie.php`にKohana_Cookieを拡張するファイルを作成して、必要な関数を加えます:

<div class="original-doc">
# Transparent Class Extension

The [cascading filesystem](files) allows transparent class extension. For instance, the class [Cookie] is defined in `SYSPATH/classes/cookie.php` as:

    class Cookie extends Kohana_Cookie {}

The default Kohana classes, and many extensions, use this definition so that almost all classes can be extended. You extend any class transparently, by defining your own class in `APPPATH/classes/cookie.php` to add your own methods.

[!!] You should **never** modify any of the files that are distributed with Kohana. Always make modifications to classes using transparent extension to prevent upgrade issues.

For instance, if you wanted to create method that sets encrypted cookies using the [Encrypt] class, you would create a file at `application/classes/cookie.php` that extends Kohana_Cookie, and adds your functions:
<div>

    <?php defined('SYSPATH') or die('No direct script access.');

    class Cookie extends Kohana_Cookie {

        /**
         * @var  mixed  default encryption instance
         */
        public static $encryption = 'default';

        /**
         * Sets an encrypted cookie.
         *
         * @uses  Cookie::set
         * @uses  Encrypt::encode
         */
         public static function encrypt($name, $value, $expiration = NULL)
         {
             $value = Encrypt::instance(Cookie::$encrpytion)->encode((string) $value);

             parent::set($name, $value, $expiration);
         }

         /**
          * Gets an encrypted cookie.
          *
          * @uses  Cookie::get
          * @uses  Encrypt::decode
          */
          public static function decrypt($name, $default = NULL)
          {
              if ($value = parent::get($name, NULL))
              {
                  $value = Encrypt::instance(Cookie::$encryption)->decode($value);
              }

              return isset($value) ? $value : $default;
          }

    } // End Cookie
</div>
こうしてから`Cookie::encrypt('secret', $data)` を呼び出せば、`$data = Cookie::decrypt('secret')`で復号できる暗号化されたクッキーが作成されるでしょう。

<div class="original-doc">
Now calling `Cookie::encrypt('secret', $data)` will create an encrypted cookie which we can decrypt with `$data = Cookie::decrypt('secret')`.
</div>

## どのように動作するのか {#how-it-works}

この働きを理解するために、通常なにが起きているのかを見てみましょう。Cookieクラスを使用するとき [Kohana::autoload] は`classes/cookie.php`を [カスケーディングファイルシステム](files)から検索します。検索は`application`を見て、次に各`modules`を見て、その次に `system`を見ます。そのファイルは `system`で見つかり、インクルードされます。当然ながら`system/classes/cookie.php` は`Kohana_Cookie`を拡張したただの空のクラスです。今度は`classes/kohana/cookie.php`を捜すために、再度[Kohana::autoload]が呼び出され、それは`system`で見つかります。

あなたが`application/classes/cookie.php`においてCookieクラスに透過的拡張を加えたときに、このファイルは本質的に`system/classes/cookie.php`に取って代わります。このときは[Kohana::autoload]が`classes/cookie.php`を検索して`application`でそれを見つけ、systemにあるクラスの替わりにそれをインクルードしたCookieクラスを使用したからです。

<div class="original-doc">
## How it works

To understand how this works, let's look at what happens normally.  When you use the Cookie class, [Kohana::autoload] looks for `classes/cookie.php` in the [cascading filesystem](files).  It looks in `application`, then each module, then `system`. The file is found in `system` and is included.  Of coures, `system/classes/cookie.php` is just an empty class which extends `Kohana_Cookie`.  Again, [Kohana::autoload] is called this time looking for `classes/kohana/cookie.php` which it finds in `system`.

When you add your transparently extended cookie class at `application/classes/cookie.php` this file essentially "replaces" the file at `system/classes/cookie.php` without actually touching it.  This happens because this time when we use the Cookie class [Kohana::autoload] looks for `classes/cookie.php` and finds the file in `application` and includes that one, instead of the one in system.
</div>

<div class="original-doc">
## Example: changing [Cookie] settings

If you are using the [Cookie](cookies) class, and want to change a setting, you should do so using transparent extension, rather than editing the file in the system folder.  If you edit it directly, and in the future you upgrade your Kohana version by replacing the system folder, your changes will be reverted and your cookies will probably be invalid.  Instead, create a cookie.php file either in `application/classes/cookie.php` or a module (`MODPATH/<modulename>/classes/cookie.php`).
</div>


	class Cookie extends Kohana_Cookie {
	
		// Set a new salt
		public $salt = "some new better random salt phrase";
		
		// Don't allow javascript access to cookies
		public $httponly = TRUE;
		
	}

## Example: TODO: an example

Just post the code and brief description of what function it adds, you don't have to do the "How it works" like above.

## Example: TODO: something else

Just post the code and brief description of what function it adds, you don't have to do the "How it works" like above.

## More examples

TODO: Provide some links to modules on github, etc that have examples of transparent extension in use.

<div class="original-doc">
## Multiple Levels of Extension

If you are extending a Kohana class in a module, you should maintain transparent extensions. In other words, do not include any variables or function in the "base" class (eg. Cookie). Instead make your own namespaced class, and have the "base" class extend that one. With our Encrypted cookie example we can create `MODPATH/mymod/encrypted/cookie.php`:
</div>

	class Encrypted_Cookie extends Kohana_Cookie {

		// Use the same encrypt() and decrypt() methods as above

	}

<div class="original-doc">
And create `MODPATH/mymod/cookie.php`:
</div>

	class Cookie extends Encrypted_Cookie {}

</div>
<div class="original-doc">
This will still allow users to add their own extension to [Cookie] while leaving your extensions intact. To do that they would make a cookie class that extends `Encrypted_Cookie` (rather than `Kohana_Cookie`) in their application folder.
</div>