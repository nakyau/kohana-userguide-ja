# 3.1.xからの移行 {#migration-from-3-1-x}

## 設定 {#config}

設定システムはさらに柔軟に書き直されました。もっとも大きな変更は、設定グループは全てのreader間でマージされるということです。ですから（データベースの接続文字列やemailの送信元アドレスのような）1つの設定値をオーバーライドでき、その設定ファイルのグループからその他の設定を継承して、それを別々の設定ソース（環境設定ファイル、データベースファイル、カスタムファイル）に格納できます。

他の点としては、主な公式APIは同じように動作すべきですが、大きな変化は`Kohana::config()`から`Kohana::$config->load()`を使うように推移していることです。`Kohana::$config`は`Config`のインスタンスです。

`Config::load()`は`Kohana::config()`とほぼ同様に動作します。例:
<div class="original-doc">
# Migrating from 3.1.x

## Config

The configuration system has been rewritten to make it more flexible. The most significant change is that config groups are now merged across all readers, similar to how files cascade within the cascading filesystem. Therefore you can now override a single config value (perhaps a database connection string, or a 'send-from' email address) and store it in a separate config source (environment config file / database / custom) while inheriting the remaining settings from that group from your application's configuration files.

In other respects, the majority of the public API should still operate in the same way, however the one major change is the transition from using `Kohana::config()` to `Kohana::$config->load()`, where `Kohana::$config` is an instance of `Config`.

`Config::load()` works almost identically to `Kohana::config()`, e.g.:
</div>


	Kohana::$config->load('dot.notation')
	Kohana::$config->load('dot')->notation

あなたのプロジェクトで簡単に `Kohana::config`/`Kohana::$config->load` を検索/置換するには、これを修正するべきです。

設定ソースの用語はさらに変更されました。3.2より前は設定は、読み込みも書き込みも"Config Readers"からロードされました。
3.2では**Config Readers**と*Config Writers*があり、どちらも**Config Source**型です。

**Config Reader**は組み込まれている`Kohana_Config_Reader`インターフェースによって組み込まれます。
同様に**Config Writer**組み込まれている`Kohana_Config_Writer`インターフェースによって組み込まれます。

<div class="original-doc">
A simple find/replace for `Kohana::config`/`Kohana::$config->load` within your project should fix this.

The terminology for config sources has also changed.  Pre 3.2 config was loaded from "Config Readers" which both
read and wrote config.  In 3.2 there are **Config Readers** and **Config Writers**, both of which are a type of 
**Config Source**.

A **Config Reader** is implemented by implementing the `Kohana_Config_Reader` interface; similarly a **Config Writer**
is implemented by implementing the `Kohana_Config_Writer` interface.

e.g. for Database:
</div>

	class Kohana_Config_Database_Reader implements Kohana_Config_Reader
	class Kohana_Config_Database_Writer extends Kohana_Config_Database_Reader implements Kohana_Config_Writer

強制されませんが、規約では作成者がConfig_Readerを拡張します。

後方互換を維持するため、設定ソースを読み込むときにdb/fileソースのために、ソースのReader/Writerを拡張した空のクラスが提供されます。

例
<div class="original-doc">
Although not enforced, the convention is that writers extend config readers.

To help maintain backwards compatability when loading config sources empty classes are provided for the db/file sources
which extends the source's reader/writer.

e.g.
</div>

	class Kohana_Config_File extends Kohana_Config_File_Reader
	class Kohana_Config_Database extends Kohana_Config_Database_Writer

Kohana 3.2では`Request_Client_External`は外部リクエストを処理するために3つの個別のドライバーを持っています。

 - `Request_Client_Curl` は既定のドライバーです。PHPのCurl拡張を使用します。
 - `Request_Client_HTTP` はPECL HTTP 拡張を使用します。
 - `Request_Client_Stream` はPHPへのネイティヴストリームを使い、拡張を必要としません。これは他のドライバーより遅くなります。

特に定めのない限り、全ての外部リクエストに`Request_Client_Curl`が使われます。これは全ての外部リクエストまたは個々のリクエストに置いて変更できます。

全てのリクエストを横断して外部リクエストドライバーをセットするには、`application/bootstrap.php`に下記の記述を加えます。

    // 外部リクエストにPECL HTTPを使うドライバをセット
    Request_Client_External::$client = 'Request_Client_HTTP';

あるいは、指定のクライアントを個々の外部要求リクエストにセットすることもできます。

<div class="original-doc">
## External requests

In Kohana 3.2, `Request_Client_External` now has three separate drivers to handle external requests;

 - `Request_Client_Curl` is the default driver, using the PHP Curl extension
 - `Request_Client_HTTP` uses the PECL HTTP extension
 - `Request_Client_Stream` uses streams native to PHP and requires no extensions. However this method is slower than the alternatives.

Unless otherwise specified, `Request_Client_Curl` will be used for all external requests. This can be changed for all external requests, or for individual requests.

To set an external driver across all requests, add the following to the `application/bootstrap.php` file;

    // Set all external requests to use PECL HTTP
    Request_Client_External::$client = 'Request_Client_HTTP';
</div>

<div class="original-doc">
Alternatively it is possible to set a specific client to an individual Request.

    // Set the Stream client to an individual request and
    // Execute
    $response = Request::factory('http://kohanaframework.org')
        ->client(new Request_Client_Stream)
        ->execute();
</div>

## HTTPキャッシュ制御 {#http-cache-control}

Kohana 3.1はRFC 2616に完全に準拠し、レスポンスの透過的なキャッシュを提供するHTTPキャッシュ制御を導入しました。

[!!] HTTPキャッシュはKohanaの全てのバーションにおいてキャッシュモジュールが有効であることが求められます。

<div class="original-doc">
## HTTP cache control

Kohana 3.1 introduced HTTP cache control, providing RFC 2616 fully compliant transparent caching of responses. Kohana 3.2 builds on this moving all caching logic out of `Request_Client` into `HTTP_Cache`.

[!!] HTTP Cache requires the Cache module to be enabled in all versions of Kohana!
</div>

Kohana 3.1ではHTTPキャッシュは以下のようにして有効にします。

    // リクエストのキャッシュを設定
    $request = Request::factory('foo/bar', Cache::instance('memcache'));

    // コントローラーにおいて、レスポンスセットのキャッシュ制御を保証する
    // これはリクエストを1時間キャッシュする
    $this->response->headers('cache-control', 
        'public, max-age=3600');
<div class="original-doc">
In Kohana 3.1, HTTP caching was enabled doing the following;

    // Apply cache to a request
    $request = Request::factory('foo/bar', Cache::instance('memcache'));

    // In controller, ensure response sets cache control,
    // this will cache the response for one hour
    $this->response->headers('cache-control', 
        'public, max-age=3600');
</div>

Kohana 3.2, HTTP では少し違います

    // リクエストのキャッシュを設定
    $request = Request::factory('foo/bar',
        HTTP_Cache::factory('memcache'));

    // コントローラーにおいて、レスポンスセットのキャッシュ制御を保証する
    // これはリクエストを1時間キャッシュする
    $this->response->headers('cache-control', 
        'public, max-age=3600');
<div class="original-doc">
In Kohana 3.2, HTTP caching is enabled slightly differently;

    // Apply cache to request
    $request = Request::factory('foo/bar',
        HTTP_Cache::factory('memcache'));

    // In controller, ensure response sets cache control,
    // this will cache the response for one hour
    $this->response->headers('cache-control', 
        'public, max-age=3600');
</div>

## コントローラーのアクションパラメータ {#controller-action-parameters}

Kohana 3.1ではコントローラーのアクションパラメータは多いに非難されました。3.2ではそれらの振る舞いは削除されました。このようなコードがあったら、

<div class="original-doc">
## Controller Action Parameters

In 3.1, controller action parameters were deprecated. In 3.2, these behavior has been removed. If you had any code like:
</div>
	public function action_index($id)
	{
		// ... code
	}
<div class="original-doc">
You'll need to change it to:
</div>
以下のように変更する必要があります:
<div class="original-doc">
	public function action_index()
	{
		$id = $this->request->param('id');

		// ... code
	}
</div>
## Formクラス {#form-class}

あなたがForm::open()を使用していたのならば、そのデフォルトの振る舞いは変更されています。
それはかつてはデフォルトで（あなたのサイトの）"/"になりました。しかし現在は空のパラメータは現在のURLを指すようになります。
<div class="original-doc">
## Form Class

If you used Form::open(), the default behavior has changed. It used to default to "/" (your home page), but now an empty parameter will default to the current URI.
</div>
