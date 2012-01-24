# セッション {#sessions}

Kohanaはクッキーでもセッションでも簡単に使えるクラスを提供します。セッションとクッキーの両方において高いレベルで同様に機能します。それらは開発者が特定のクライアントについての一時的または永続的な情報を後に（通常はリクエスト間で何かを持続させる目的で）取得するために格納することを可能にします。

セッションは一時的またはプライベートなデータを格納するために使用されます。取り扱いに非常な注意を要するデータは[Session]クラスを"database"アダプターまたは"native"アダプターと共に使用して格納するべきです。"cookie"アダプターを使用するときはセッションは常に暗号化されているべきです。

[!!] セッション変数についてのベストプラクティスについては [the seven deadly sins of sessions](http://lists.nyphp.org/pipermail/talk/2006-December/020358.html)を見てください。

<div class="original-doc">
# Sessions

Kohana provides classes that make it easy to work with both cookies and sessions. At a high level both sessions and cookies provide the same functionality. They allow the developer to store temporary or persistent information about a specific client for later retrieval, usually to make something persistent between requests.

Sessions should be used for storing temporary or private data.  Very sensitive data should be stored using the [Session] class with the "database" or "native" adapters. When using the "cookie" adapter, the session should always be encrypted.

[!!] For more information on best practices with session variables see [the seven deadly sins of sessions](http://lists.nyphp.org/pipermail/talk/2006-December/020358.html).
</div>
## データの登録、取得、削除 {#storing-retrieving-and-deleting-data}

[Cookie] と [Session] はデータ格納のための互いによく似たAPIを提供します。両者の主な違いはSessionはオブジェクトを使用してアクセスされるのに対し、Cookieはスタティッククラスからアクセスされることです。

セッションインスタンスへのアクセスは[Session::instance]メソッドによって行われます:

    // Sessionのインスタンスを取得
    $session = Session::instance();

<div class="original-doc">
## Storing, Retrieving, and Deleting Data

[Cookie] and [Session] provide a very similar API for storing data. The main difference between them is that sessions are accessed using an object, and cookies are accessed using a static class.

Accessing the session instance is done using the [Session::instance] method:

    // Get the session instance
    $session = Session::instance();
</div>

セッションを使用するときは, you can also get all of the current session data using the [Session::as_array] method:
セッションを使用する場合、[Session::as_array]を使用して、現在のセッションデータの全てを取得することもできます:

    // 全てのセッションデータを配列として作成
    $data = $session->as_array();


$session->as_array()は標準のPHPでデータをセット/取得するための`$_SESSION`グローバルをオーバーロードするのにも使えます。

    // $_SESSION をセッションデータでオーバーロード
    $_SESSION =& $session->as_array();
    
    // セッションにデータをセットする
    $_SESSION[$key] = $value;

<div class="original-doc">
When using sessions, you can also get all of the current session data using the [Session::as_array] method:

    // Get all of the session data as an array
    $data = $session->as_array();

You can also use this to overload the `$_SESSION` global to get and set data in a way more similar to standard PHP:

    // Overload $_SESSION with the session data
    $_SESSION =& $session->as_array();
    
    // Set session data
    $_SESSION[$key] = $value;
</div>

### データの登録 {#storing-data}

セッションまたはクッキーにデータを格納するには`set`メソッドを使用します:

    // セッションにデータを格納
    $session->set($key, $value);
    // セッションにデータを格納
    Session::instance()->set($key, $value);

    // ユーザIDを格納
    $session->set('user_id', 10);

<div class="original-doc">
### Storing Data

Storing session or cookie data is done using the `set` method:

    // Set session data
    $session->set($key, $value);
	// Or
	Session::instance()->set($key, $value);

    // Store a user id
    $session->set('user_id', 10);
</div>
### データの取得 {#retriving-data}

セッションまたはクッキーのデータを取得するには`get`メソッドを使用します:

    // セッションデータを取得
    $data = $session->get($key, $default_value);

    // ユーザIDを取得
    $user = $session->get('user_id');

### データの取得 {#retrieving-data}

セッションまたはクッキーのデータを取得するには`get`を使用します:

    // セッションデータを取得
    $data = $session->get($key, $default_value);

    // ユーザIDを取得
    $user = $session->get('user_id');


<div class="original-doc">
### Retrieving Data

Getting session or cookie data is done using the `get` method:

    // Get session data
    $data = $session->get($key, $default_value);

    // Get the user id
    $user = $session->get('user_id');
</div>

### データの削除 {#deleting-data}

セッションまたはクッキーのデータを削除するには`delete`メソッドを使用します:

    // セッションデータを削除
    $session->delete($key);


    // ユーザIDを削除
    $session->delete('user_id');

<div class="original-doc">
### Deleting Data

Deleting session or cookie data is done using the `delete` method:

    // Delete session data
    $session->delete($key);


    // Delete the user id
    $session->delete('user_id');
</div>

## セッションの設定 {#session-configuration}

アプリケーションを公開する前に必ずこれらの設定をチェックしてください。セッションの設定の多くはアプリケーションのセキュリティに直接影響します。

<div class="original-doc">
## Session Configuration

Always check these settings before making your application live, as many of them will have a direct affect on the security of your application.
</div>
## セッションアダプター {#session-adapters}

[Session]クラスを作成またはそれにアクセスする場合にどのセッションアダプター/ドライバーを共に使用するか決めることができます。使用できるセッションアダプターは:

<div class="original-doc">
## Session Adapters

When creating or accessing an instance of the [Session] class you can decide which session adapter or driver you wish to use. The session adapters that are available to you are:
</div>
Native
: ウェブサーバの既定の場所にセッションデータを格納します。ストレージの場所は`php.ini`の[session.save_path](http://php.net/manual/session.configuration.php#ini.session.save-path) または[ini_set](http://php.net/ini_set)で定義されます。

Database
: [Session_Database]クラスを使用してデータベースのテーブルにセッションデータを格納します。[Database]モジュールが有効になっている必要があります。

Cookie
: [Cookie]クラスを使用してクッキーにセッションデータを格納します。 **このアダプターを使用する場合にはセッションサイズは4キロバイトの制限があります。そしてデータは暗号化するべきです**

<div class="original-doc">
Native
: Stores session data in the default location for your web server. The storage location is defined by [session.save_path](http://php.net/manual/session.configuration.php#ini.session.save-path) in `php.ini` or defined by [ini_set](http://php.net/ini_set).

Database
: Stores session data in a database table using the [Session_Database] class. Requires the [Database] module to be enabled.

Cookie
: Stores session data in a cookie using the [Cookie] class. **Sessions will have a 4KB limit when using this adapter, and should be encrypted.**
</div>
デフォルトのアダプターは[Session::$default]の値によって変更できます。デフォルトのアダプターは"native"です。

デフォルトのアダプターを使用してセッションにアクセスするには、ただ[Session::instance()]を呼び出します。デフォルト意外のアダプターを使用してセッションにアクセスする場合には、アダプターの名前を`instance()`に引数として渡します。例えば、`Session::instance('cookie')`です。

<div class="original-doc">
The default adapter can be set by changing the value of [Session::$default]. The default adapter is "native".

To access a Session using the default adapter, simply call [Session::instance()].  To access a Session using something other than the default, pass the adapter name to `instance()`, for example: `Session::instance('cookie')`
</div>
### セッションアダプターの設定 {#session-adapter-settings}

`APPPATH/config/session.php`にセッションの設定ファイルを作成することで各セッションアダプターごとに設定値を適用することができます。以下の設定ファイルのサンプルは各アダプターごとにセッションの設定を定義しています。

[!!] Cookieアダプターにおいて "lifetime"に設定された"0"は、ブラウザを閉じたときにセッションが有効期限切れになることを意味します。

<div class="original-doc">
### Session Adapter Settings

You can apply configuration settings to each of the session adapters by creating a session config file at `APPPATH/config/session.php`. The following sample configuration file defines all the settings for each adapter:

[!!] As with cookies, a "lifetime" setting of "0" means that the session will expire when the browser is closed.
</div>

    return array(
        'native' => array(
            'name' => 'session_name',
            'lifetime' => 43200,
        ),
        'cookie' => array(
            'name' => 'cookie_name',
            'encrypted' => TRUE,
            'lifetime' => 43200,
        ),
        'database' => array(
            'name' => 'cookie_name',
            'encrypted' => TRUE,
            'lifetime' => 43200,
            'group' => 'default',
            'table' => 'table_name',
            'columns' => array(
                'session_id'  => 'session_id',
        		'last_active' => 'last_active',
        		'contents'    => 'contents'
            ),
            'gc' => 500,
        ),
    );


#### Native Adapter

データ型   | 設定       | 説明                                              | 規定値
----------|-----------|---------------------------------------------------|-----------
`string`  | name      | セッションの名前                                    | `"session"`
`integer` | lifetime  | セッションの生存時間の秒数                            | `0`

<div class="original-doc">
Type      | Setting   | Description                                       | Default
----------|-----------|---------------------------------------------------|-----------
`string`  | name      | name of the session                               | `"session"`
`integer` | lifetime  | number of seconds the session should live for     | `0`
</div>


#### Cookie Adapter

データ型   | 設定       | 説明                                              | 規定値
----------|-----------|---------------------------------------------------|-----------
`string`  | name      | セッションデータを格納するのに使用されるクッキーの名前    | `"session"`
`boolean` | encrypted | [Encrypt]を使用してセッションデータを暗号化するか？     | `FALSE`
`integer` | lifetime  | セッションの生存時間の秒数                            | `0`
<div class="original-doc">
Type      | Setting   | Description                                       | Default
----------|-----------|---------------------------------------------------|-----------
`string`  | name      | name of the cookie used to store the session data | `"session"`
`boolean` | encrypted | encrypt the session data using [Encrypt]?         | `FALSE`
`integer` | lifetime  | number of seconds the session should live for     | `0`
</div>


#### Database Adapter
データ型   | 設定       | 説明                                              | 規定値
----------|-----------|---------------------------------------------------|-----------
`string`  | group     | [Database::instance] クループの名前                 | `"default"`
`string`  | table     | セッションデータを格納するテーブル名                   | `"sessions"`
`array`   | columns   | カラム名の連想配列                                   | `array`
`integer` | gc        | 1:x の割合でガーベッジコレクションを実行               | `500`
`string`  | name      | セッションデータを格納するのに使用されるクッキーの名前    | `"session"`
`boolean` | encrypted | [Encrypt]を使用してセッションデータを暗号化するか？     | `FALSE`
`integer` | lifetime  | セッションの生存時間の秒数                            | `0`

<div class="original-doc">
Type      | Setting   | Description                                       | Default
----------|-----------|---------------------------------------------------|-----------
`string`  | group     | [Database::instance] group name                   | `"default"`
`string`  | table     | table name to store sessions in                   | `"sessions"`
`array`   | columns   | associative array of column aliases               | `array`
`integer` | gc        | 1:x chance that garbage collection will be run    | `500`
`string`  | name      | name of the cookie used to store the session data | `"session"`
`boolean` | encrypted | encrypt the session data using [Encrypt]?         | `FALSE`
`integer` | lifetime  | number of seconds the session should live for     | `0`
</div>

##### テーブル定義 {#table-schema}

Databaseアダプターを使用する場合は、データベースにセッションを格納するテーブルを作成する必要があります。これはデフォルトの定義です:

<div class="original-doc">
##### Table Schema

You will need to create the session storage table in the database. This is the default schema:
</div>
    CREATE TABLE  `sessions` (
        `session_id` VARCHAR(24) NOT NULL,
        `last_active` INT UNSIGNED NOT NULL,
        `contents` TEXT NOT NULL,
        PRIMARY KEY (`session_id`),
        INDEX (`last_active`)
    ) ENGINE = MYISAM;

##### テーブルのカラム {#table-columns}

カラム名はレガシーのセッションテーブルに接続した際にそのデータベースの定義に存在するもの変更できます。デフォルト値はキーの値と同じ名前です。

session_id
: "id" カラムの列名

last_active
: 最後にセッションを更新した時間のUNIXタイムスタンプ

contents
: シリアライズされたセッションデータが文字列で格納される。（オプションにより暗号化）
<div class="original-doc">
##### Table Columns

You can change the column names to match an existing database schema when connecting to a legacy session table. The default value is the same as the key value.

session_id
: the name of the "id" column

last_active
: UNIX timestamp of the last time the session was updated

contents
: session data stored as a serialized string, and optionally encrypted
</div>
