# Kohanaの共有 {#sharing-kohana}

Kohanaはフロントコントローラーパターンに従っています。これは全てのリクエストが`index.php`に送信されることを意味し、そのファイルシステムは設定が容易です。`index.php`の内部で`$applocation`,`$modules`,`$system`のパスを変更可能です。

[!!] それらがフロントコントローラーを使用せずにアクセスされることを防ぐために全てのKohanaファイルの一番上にセキュリティチェックがあります。さらに`.htaccess`ファイルは同様にそれらのファイルを保護します。WEBからアクセスすることができない別のセキュリティレイヤーにapplication,modules,systemディレクトリを移動することができますが、これはオプションです。

`$application`変数にはアプリケーションファイルを格納するディレクトリをセットできます。デフォルトでは`application`です。`$modules`変数にはモジュールファイルを格納するディレクトリをセットできます。`$system`変数にはKohanaのファイルを格納するディレクトリをセットできます。この3つのディレクトリはどこにでも移動することができます。

例えば、デフォルトではディレクトリはこのようにセットされています。

<div class="original-doc">
# Sharing Kohana

Because Kohana follows a [front controller] pattern, which means that all requests are sent to `index.php`, the filesystem is very configurable.  Inside of `index.php` you can change the `$application`, `$modules`, and `$system` paths.

[!!] There is a security check at the top of every Kohana file to prevent it from being accessed without using the front controller.  Also, the `.htaccess` file should protect those folders as well.  Moving the application, modules, and system directories to a location that cannot be accessed vie web can add another layer of security, but is optional.   

The `$application` variable lets you set the directory that contains your application files. By default, this is `application`. The `$modules` variable lets you set the directory that contains module files. The `$system` variable lets you set the directory that contains the default Kohana files. You can move these three directories anywhere.

For instance, by default the directories are set up like this:
</div>

    www/
        index.php
        application/
        modules/
        system/

このようにしてディレクトリをドキュメントルートの外側に移動できます。
<div class="original-doc">
You could move the directories out of the web root so they look like this:
</div>

    application/
    modules/
    system/
    www/
        index.php

設定を変更するときは`index.php`をこうします。
<div class="original-doc">
Then you would need to change the settings in `index.php` to be:
</div>

    $application = '../application';
    $modules     = '../modules';
    $system      = '../system';


## systemとmoduleの共有 {#sharing-system-and-modules}

1つの手順を踏むことで、複数のKohanaアプリケーションを同じsytemとmodileフォルダに向けることが出来ます。例（これは一例です。あなたの望むように変更できます。）
<div class="original-doc">
## Sharing system and modules

To take this a step further, we could point several kohana apps to the same system and modules folders.  For example (and this is just an example, you could arrange these anyway you want):
</div>

	apps/
		foobar/
			application/
			www/
		bazbar/
			application/
			www/
	kohana/
		3.0.6/
		3.0.7/
		3.0.8/
	modules/

`index.php`をこのように変更する必要があります。
<div class="original-doc">
And you would need to change the settings in `index.php` to be:
</div>

	$application = '../application';
	$system      = '../../../kohana/3.0.6';
	$modules     = '../../../kohana/modules';

この方法を使って各アプリケーションを1つのKohanaファイルに向けることが出来ます。そして新しいバージョンを加えて、あなたのアプリケーションを新バージョンに向けるように`index.php`を編集することで、素早くアップデートできます。
<div class="original-doc">
Using this method each app can point to a central copy of kohana, and you can add a new version, and quickly update your apps to point to the new version by editing their `index.php` files.
</div>