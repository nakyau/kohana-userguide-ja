# リクエストフロー {#request-flow}

どのアプリケーションも同じフローに従います:

1. アプリケーションは`index.php`から開始します。
	1. application、moduleとsystemのパスがをセットします。 (`APPPATH`、`MODPATH`と`SYSPATH`)
	2. エラーレポートのレベルをセットします。
	3. インストールファイルが存在すれば、読み込まれます。
	4. ブートストラップファイル`APPPATH/bootstrap.php`がインクルードされます。
2. `bootstrap.php`の中では:
	6. [Kohana] クラスがロードされます。
	7.  which sets up エラーハンドル、キャッシュ、ログをセットアップする[Kohana::init] が呼ばれます。
	8. [Kohana_Config] リーダーと [Kohana_Log] ライターが組み込まれます。
	9. アプリケーションモジュールを有効化する[Kohana::modules] が呼ばれます。
	    * モジュールパスは [cascading filesystem](files)に追加されます。
		* `init.php`ファイルが存在すれば、モジュールごとにインクルードされます。 
	    * The `init.php` ファイルはルートの追加を含む、付加的なの環境設定を処理できます。
	10. [Route::set] は[ルート](routing)を定義するために複数回呼ばれます。
	11. リクエストの処理を開始するために[Request::instance] が呼ばれます。
		1. 各ルートについてマッチするルートが見つかり、ルートがセットされるかをチェックします。
		2. コントローラーインスタンスを作成し、リクエストを渡します。
		3. [Controller::before] メソッドを呼び出します。
		4. コントローラーのアクションを呼び出し、リクエストレスポンスを作成します。
		5. [Controller::after] を呼び出します。
		    * [HMVC サブリクエスト](requests)が使用されるとき、上記の5つの行程は複数回繰り返されます。
3. アプリケーションのフローからindex.phpに戻ります。
	12. [リクエスト](requests) レスポンスが表示されます。
<div class="original-doc">
# Request Flow

Every application follows the same flow:

1. Application starts from `index.php`.
	1. The application, module, and system paths are set. (`APPPATH`, `MODPATH`, and `SYSPATH`)
	2. Error reporting levels are set.
	3. Install file is loaded, if it exists.
	4. The bootstrap file, `APPPATH/bootstrap.php`, is included.
2. Once we are in `bootstrap.php`:
	6. The [Kohana] class is loaded.
	7. [Kohana::init] is called, which sets up error handling, caching, and logging.
	8. [Kohana_Config] readers and [Kohana_Log] writers are attached.
	9. [Kohana::modules] is called to enable additional modules.
	    * Module paths are added to the [cascading filesystem](files).
		* Includes each module's `init.php` file, if it exists. 
	    * The `init.php` file can perform additional environment setup, including adding routes.
	10. [Route::set] is called multiple times to define the [application routes](routing).
	11. [Request::instance] is called to start processing the request.
		1. Checks each route that has been set until a match is found.
		2. Creates the controller instance and passes the request to it.
		3. Calls the [Controller::before] method.
		4. Calls the controller action, which generates the request response.
		5. Calls the [Controller::after] method.
		    * The above 5 steps can be repeated multiple times when using [HMVC sub-requests](requests).
3. Application flow returns to index.php
	12. The main [Request] response is displayed
</div>
