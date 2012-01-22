# Tipsとよくある間違い {#tips-and-common-mistakes}

これはTipsとよくある間違いまたはあなたが出会うであろうエラーを集めたものです。
<div class="original-doc">
# Tips and Common Mistakes

This is a collection of tips and common mistakes or errors you may encounter. 
</div>
## `system`フォルダを決して編集しないこと！ {#never-edit-system-folder}

（ほとんどの場合）システムフォルダを編集してはなりません。
systemとmoduleにあるファイルに加えたいどんな変更も[カスケーディングファイルシステム](files)と[透過的エクステンション](extension)によっておこなうことができますし、これはあなたがKohanaのバージョンを更新するときにも変わることがありません。

<div class="original-doc">
## Never edit the `system` folder!

You should (almost) never edit the system folder.  Any change you want to make to files in system and modules can be made via the [cascading filesystem](files) and [transparent extension](extension) and won't break when you try to update your Kohana version.  
</div>
## 1つのルートを何もかもに使おうとしないこと {#dont-try-and-use-on-route-for-everything}

Kohana 3の [ルート](routing) はとても強力で柔軟です。
あなたのアプリケーションに必要なだけルートを使うことをためらわないでください。

<div class="original-doc">
## Don't try and use one route for everything

Kohana 3 [routes](routing) are very powerful and flexible, don't be afraid to use as many as you need to make your app function the way you want!
</div>
## 複数のルートを処理する {#handling-lots-of-routes}

時としてあなたのアプリケーションは複数のルートを持ち、それら全てbootstrap.phpで管理するのが不可能であるほどに、とても複雑なものになります。そういう場合は、APPPATHに`routes.php`ファイルを作成し、bootstrap.phpからrequireします: `require_once APPPATH.'routes'.EXT;`

<div class="original-doc">
## Handling lots of routes

Sometimes your application is sufficiently complex that you have many routes and it becomes unmanageable to put them all in bootstrap.php. If this is the case, simply make a `routes.php` file in APPPATH and require that in your bootstrap: `require_once APPPATH.'routes'.EXT;`
</div>


## Reflection_Exception

サイトをセットアップしたときReflection_Exceptionが起きたなら、それはほとんど[Kohana::init]で起きており、'base_url'の設定が間違っているはずです。
もし`base_url`が正しいなら、[routes](routing)になにか間違いがあるかもしれません。

<div class="original-doc">
If you get a Reflection_Exception when setting up your site, it is almost certainly because your [Kohana::init] 'base_url' setting is wrong.  If your base url is correct something is probably wrong with your [routes](routing).
</div>

	ReflectionException [ -1 ]: Class controller_<something> does not exist
	// where <something> is part of the url you entered in your browser

### 解決方法  {#reflection-exception-solution}

[Kohana::init] 'base_url'を正しく設定する。base_urlはindex.phpへの相対パスかウェブサーバのドキュメントルートになるべきです。

<div class="original-doc">
### Solution  {#reflection-exception-solution}

Set your [Kohana::init] 'base_url' to the correct setting. The base url should be the path to your index.php file relative to the webserver document root.
</div>

## ORM/Session __sleep() のバグ {#orm-session-slieep-bug}

phpにfatalエラーを起こした後にセッションを破壊するバグがあります。
公開サーバはfatalエラーをキャッチするべきではありませんので、このバグはおよそ開発中になにか間違いをしておこります。
この不具合は、fatalエラーを起こした後のページ読み込みにおいてデータベース接続エラーが起きます。
そして、その後全てのページ読み込みにおいて以下のエラーを表示します。

<div class="original-doc">
## ORM/Session __sleep() bug

There is a bug in php which can corrupt your session after a fatal error.  A production server shouldn't have uncaught fatal errors, so this bug should only happen during development, when you do something stupid and cause a fatal error.  On the next page load you will get a database connection error, then all subsequent page loads will display the following error:
</div>

	ErrorException [ Notice ]: Undefined index: id
	MODPATH/orm/classes/kohana/orm.php [ 1308 ]

### 解決方法   {#orm-session-sleep-solution}

これを修正するには, ドメインに対するセッションをクリアするためにクッキーを削除します。
これは公開サーバでは決して起こりません。ですから、顧客に対してクッキーを削除する方法を説明する必要はありません。
詳細は[この課題についてのディスカッション](http://dev.kohanaframework.org/issues/3242) を見てください。

<div class="original-doc">
### Solution   {#orm-session-sleep-solution}

To fix this, clear your cookies for that domain to reset your session.  This should never happen on a production server, so you won't have to explain to your clients how to clear their cookies.  You can see the [discussion on this issue](http://dev.kohanaframework.org/issues/3242) for more details.
</div>
