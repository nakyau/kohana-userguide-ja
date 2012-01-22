# インストール {#installation}

1. 最新の安定版(stablle)リリースパッケージを[Kohana ウェブサイト](http://kohanaframework.org/)からダウンロードします。
2. ダウンロードしたパッケージを解凍して`kohana`ディレクトリを作成します。
3. kohanaディレクトリのコンテンツをウェブサーバにアップロードします。
4. `application/bootstrap.php`　を開いて以下の変更を加えます:
	- あなたのアプリケーションにおけるデフォルトの [タイムゾーン](http://php.net/timezones) を設定する
	- `base_url` に[Kohana::init] の呼び出しで反映するkohanaフォルダの場所を、ドキュメントルートからの相対パスで設定します。
6. `application/cache` と `application/logs` ディレクトリがウェブサーバから書き込み可能になっていることを確認します。
7. ブラウザで`base_url`に設定したURLにアクセスしてインストールを確認してください。

[!!] 環境によっては、解凍する際にサブディレクトリのパーミッションが失われることがあります。 `find . -type d -exec chmod 0755 {} \;` を実行してkohanaをインストールするディレクトリのサブディレクトリのパーミッションをすべて755に変更してください。

インストールページが表示されます。エラーが報告されている場合は次の作業に進む前にエラーを解決してください。

<div class="original-doc">
# Installation

1. Download the latest **stable** release from the [Kohana website](http://kohanaframework.org/).
2. Unzip the downloaded package to create a `kohana` directory.
3. Upload the contents of this folder to your webserver.
4. Open `application/bootstrap.php` and make the following changes:
	- Set the default [timezone](http://php.net/timezones) for your application.
	- Set the `base_url` in the [Kohana::init] call to reflect the location of the kohana folder on your server relative to the document root.
6. Make sure the `application/cache` and `application/logs` directories are writable by the web server.
7. Test your installation by opening the URL you set as the `base_url` in your favorite browser.

[!!] Depending on your platform, the installation's subdirs may have lost their permissions thanks to zip extraction. Chmod them all to 755 by running `find . -type d -exec chmod 0755 {} \;` from the root of your Kohana installation.

You should see the installation page. If it reports any errors, you will need to correct them before continuing.
</div>

![Install Page](install.png "Example of install page")

環境が正しくセットアップされたことを確認したら、`install.php`をルートディレクトリから削除するかリネームしてください。
これでkohanaがインストールされ、コントローラーからのウェルカムメッセージ（Hello, World!）が表示されているはずです。
<div class="original-doc">
Once your install page reports that your environment is set up correctly you need to either rename or delete `install.php` in the root directory. Kohana is now installed and you should see the output of the welcome controller:
</div>


![Welcome Page](welcome.png "Example of welcome page")

## GitHub から Kohana 3.1をインストール {#installing-konana-31-from-github}

kohana 3.1の[ソースコード](http://github.com/kohana/kohana) は [GitHub](http://github.com) にあります。githubのソースコードを利用してインストールを行うには、まずgitをインストールしてください。 あなたの環境にgitをインストールする方法の詳細は [http://help.github.com](http://help.github.com) を訪ねてください。

[!!] gitのサブモジュールを利用したKoahanaのインストールについてのさらなる情報は, [Working with Git](tutorials/git) チュートリアルを見てください。
<div class="original-doc">
## Installing Kohana 3.1 From GitHub

The [source code](http://github.com/kohana/kohana) for Kohana 3.1 is hosted with [GitHub](http://github.com).  To install Kohana using the github source code first you need to install git.  Visit [http://help.github.com](http://help.github.com) for details on how to install git on your platform.

[!!] For more information on installing Kohana using git submodules, see the [Working with Git](tutorials/git) tutorial.
</div>