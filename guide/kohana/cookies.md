# クッキー {#cookies}

Kohanaはクッキーでもセッションでも簡単に使えるクラスを提供します。セッションとクッキーの両方において高いレベルで同様に機能します。それらは開発者が特定のクライアントについての一時的または永続的な情報を後に（通常はリクエスト間で何かを持続させる目的で）取得するために格納することを可能にします。

[クッキー](http://en.wikipedia.org/wiki/HTTP_cookie) 長期間にわたって永続化する秘密ではないデータを格納するのに使用すべきです。例えば、ユーザ設定や言語設定の保存です。[Cookie]クラスを使用してクッキーの取得と登録をしましょう。

<div class="original-doc">
# Cookies
Kohana provides classes that make it easy to work with both cookies and sessions. At a high level both sessions and cookies provide the same functionality. They allow the developer to store temporary or persistent information about a specific client for later retrieval, usually to make something persistent between requests.

[Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) should be used for storing non-private data that is persistent for a long period of time. For example storing a user preference or a language setting. Use the [Cookie] class for getting and setting cookies.

[!!] Kohana uses "signed" cookies. Every cookie that is stored is combined with a secure hash to prevent modification of the cookie.  If a cookie is modified outside of Kohana the hash will be incorrect and the cookie will be deleted.  This hash is generated using [Cookie::salt()], which uses the [Cookie::$salt] property. You must define this setting in your bootstrap.php:
</div>

	Cookie::$salt = 'foobar';

あるいは、拡張したクッキークラスをあなたのアプリケーションに定義します。
<div class="original-doc">
Or define an extended cookie class in your application:
</div>

	class Cookie extends Kohana_Cookie
	{
		public static $salt = 'foobar';
	}

セキュアな値にはsalt文字列をセットするべきです。上記の例は実践例にすぎません。

あなたが普通に`$_COOKIE`を使用することも制限しません。しかしCookieクラスと通常の`$_COOKIE`グルーバル変数をミックスして使用することはできません。なぜならば、Kohanaがクッキーを署名するのに使用するハッシュは読むことができません。そしてKahanaはそのクッキーを削除します。

<div class="original-doc">
You should set the salt to a secure value. The example above is only for demonstrative purposes.

Nothing stops you from using `$_COOKIE` like normal, but you can not mix using the Cookie class and the regular `$_COOKIE` global, because the hash that Kohana uses to sign cookies will not be present, and Kohana will delete the cookie.
</div>


## データの登録・取得・削除 {#storing-retrieving-and-delete-data}
[Cookie] と [Session] はデータ格納のための互いによく似たAPIを提供します。両者の主な違いはSessionはオブジェクトを使用してアクセスされるのに対し、Cookieはスタティッククラスからアクセスされることです。

<div class="original-doc">
## Storing, Retrieving, and Deleting Data

[Cookie] and [Session] provide a very similar API for storing data. The main difference between them is that sessions are accessed using an object, and cookies are accessed using a static class.
</div>


### データの登録 {#storing-data}

セッションまたはクッキーにデータを格納するには [Cookie::set] メソッドを使用します:

    // クッキーデータをセットする
    Cookie::set($key, $value);

    // ユーザIDをセットする
    Cookie::set('user_id', 10);


<div class="original-doc">
### Storing Data

Storing session or cookie data is done using the [Cookie::set] method:

    // Set cookie data
    Cookie::set($key, $value);

    // Store a user id
    Cookie::set('user_id', 10);

</div>

### データの取得 {#retrieving-data}

セッションまたはクッキーのデータを取得するには[Cookie::get]を使用します:

    // クッキーデータを取得
    $data = Cookie::get($key, $default_value);

    // ユーザIDを取得
    $user = Cookie::get('user_id');


<div class="original-doc">
### Retrieving Data

Getting session or cookie data is done using the [Cookie::get] method:

    // Get cookie data
    $data = Cookie::get($key, $default_value);

    // Get the user id
    $user = Cookie::get('user_id');

</div>

### データの削除 {#deleting-data}

セッションまたはクッキーのデータを削除するには [Cookie::delete] メソッドを使用します:
    // クッキーデータを削除
    Cookie::delete($key);

    // ユーザIDを削除
    Cookie::delete('user_id');

<div class="original-doc">
### Deleting Data

Deleting session or cookie data is done using the [Cookie::delete] method:
    
    // Delete cookie data
    Cookie::delete($key);

    // Delete the user id
    Cookie::delete('user_id');

</div>

## クッキーの設定 {#cookie-settings}
全てのクッキーの設定はスタティックプロパティを使用して変更されます。これらの設定を`bootstrap.php` や [透過的拡張](extension)を使用して変更することもできます。あなたのアプリケーションを公開する前に必ず設定を確認してください。クッキーの設定の多くはアプリケーションのセキュリティに直接影響します。

最も重要な設定はセキュリティ署名に使用する [Cookie::$salt] です。この値は変更して秘密にするべきです。

<div class="original-doc">
## Cookie Settings

All of the cookie settings are changed using static properties. You can either change these settings in `bootstrap.php` or by using [transparent extension](extension).  Always check these settings before making your application live, as many of them will have a direct affect on the security of your application.

The most important setting is [Cookie::$salt], which is used for secure signing. This value should be changed and kept secret:
</div>

    Cookie::$salt = 'your secret is safe with me';


[!!] この値を変更することは変更以前にセットされたクッキーを全て無効にすることになります。
<div class="original-doc">
[!!] Changing this value will render all cookies that have been set before invalid.
</div>

デフォルトではクッキーはブラウザを閉じるまで保存されています。生存時間を指定するには[Cookie::$expiration] の設定を変更します。

    // クッキーの有効期限を1週間後に設定
    Cookie::$expiration = 604800;

    // 分かりやすくするために、整数値を使用する別の方法
    Cookie::$expiration = Date::WEEK;


クッキーにアクセスすることができるパスは[Cookie::$path]で制限できます。

    // /public/* からのみクッキーを許可
    Cookie::$path = '/public/';

<div class="original-doc">
By default, cookies are stored until the browser is closed. To use a specific lifetime, change the [Cookie::$expiration] setting:

    // Set cookies to expire after 1 week
    Cookie::$expiration = 604800;

    // Alternative to using raw integers, for better clarity
    Cookie::$expiration = Date::WEEK;

The path that the cookie can be accessed from can be restricted using the [Cookie::$path] setting.

    // Allow cookies only when going to /public/*
    Cookie::$path = '/public/';
</div>

クッキーにアクセスすることができるドメインも[Cookie::$domain]を使用して制限できます。

    // www.example.com からのみクッキーを許可
    Cookie::$domain = 'www.example.com';


すべてのサブドメインからクッキーにアクセスできるようにするにはドメインの先頭に.(ドット)を付けてください。

    // example.com と *.example.com からクッキーを許可
    Cookie::$domain = '.example.com';

<div class="original-doc">
The domain that the cookie can be accessed from can also be restricted, using the [Cookie::$domain] setting.

    // Allow cookies only on the domain www.example.com
    Cookie::$domain = 'www.example.com';

If you want to make the cookie accessible on all subdomains, use a dot at the beginning of the domain.

    // Allow cookies to be accessed on example.com and *.example.com
    Cookie::$domain = '.example.com';
</div>

セキュアな(HTTPS)接続からのみクッキーへのアクセスを許可するには[Cookie::$secure]を使用します。

    // セキュアな接続のみクッキーへのアクセスを許可
    Cookie::$secure = TRUE;
    
    // 全ての接続においてクッキーへのアクセスを許可
    Cookie::$secure = FALSE;

javascriptを使用してのクッキーへのアクセスを防ぐには [Cookie::$httponly] を設定します。

    // クッキーをjavascriptからアクセスできないようにする。
    Cookie::$httponly = TRUE;

<div class="original-doc">
To only allow the cookie to be accessed over a secure (HTTPS) connection, use the [Cookie::$secure] setting.

    // Allow cookies to be accessed only on a secure connection
    Cookie::$secure = TRUE;
    
    // Allow cookies to be accessed on any connection
    Cookie::$secure = FALSE;

To prevent cookies from being accessed using Javascript, you can change the [Cookie::$httponly] setting.

    // Make cookies inaccessible to Javascript
    Cookie::$httponly = TRUE;
</div>