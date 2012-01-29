# きれいなURL {#clean-url}
URLからindex.phpを取り除く。

URLをきれいに保つために、おそらくあなたは`/index.php/`無しのURLでアプリケーションにアクセスしたいでしょう。

1. ブートストラップファイルを編集する
2. Rewriteを設定する
<div class="original-doc">
# Clean URLs

Removing `index.php` from your urls.

To keep your URLs clean, you will probably want to be able to access your app without having `/index.php/` in the URL. There are two steps to remove `index.php` from the URL.

1. Edit the bootstrap file
2. Set up rewriting
</div>

## 1. ブートストラップの設定 {#configure-bootstrap}

変更しなければならない最初のことは[Kohana::init]の`index_file`をfalseに設定することです。

	Kohana::init(array(
	      'base_url'   => '/myapp/',
	      'index_file' => FALSE,
	));

この変更は[URL::site],[URL::base]と[HTML::anchor]を使用して生成された全てのリンクのURLに"index.php"が含まれなくなります。全ての生成されたリンクは`/myapp/index.php/`ではなく`/myapp/`から始まるようになります。

<div class="original-doc">
## 1. Configure Bootstrap

The first thing you will need to change is the `index_file` setting of [Kohana::init] to false:
　
    Kohana::init(array(
        'base_url'   => '/myapp/',
        'index_file' => FALSE,
    ));

This change will make it so all of the links generated using [URL::site], [URL::base], and [HTML::anchor] will no longer include "index.php" in the URL. All generated links will start with `/myapp/` instead of `/myapp/index.php/`.
</div>

## 2. URL Rewriting
Rewriteを設定することは違う方法で行われ、あなたのWebサーバに依存します。
RewriteはURLをindex.phpに渡されるようにします。
<div class="original-doc">
Enabling rewriting is done differently, depending on your web server.

Rewriting will make it so urls will be passed to index.php.
</div>

## Apache
`example.htaccess` をただの `.htaccess` にリネームして、`RewriteBase`の行を[Kohana::init]の設定の`base_url`にマッチするように変更します。

    RewriteBase /myapp/

`.htaccess file`によるURLの書き換えはファイルがサーバに存在する場合を除いて全てのリクエストをindex.phpに書き換えます。（ですからcss,images,faviconなどは依然として普通に扱われます。）多くの場合、これだけであなたの作業は終わりです！

<div class="original-doc">
Rename `example.htaccess` to only `.htaccess` and alter the `RewriteBase` line to match the `base_url` setting from your [Kohana::init]

    RewriteBase /myapp/

The rest of the `.htaccess file` rewrites all requests through index.php, unless the file exists on the server (so your css, images, favicon, etc. are still loaded like normal).  In most cases, you are done!
</div>

### 失敗！ {#failed}
もし"Internal Server Error" や "No input file specified" のエラーが起きたら、変更してください。

    RewriteRule ^(?:application|modules|system)\b - [F,L]

替わりにスラッシュを試してみます。

    RewriteRule ^(application|modules|system)/ - [F,L]

うまく動かなければ、変更。

    RewriteRule .* index.php/$0 [PT]

より単純なものにする。

    RewriteRule .* index.php [PT]


<div class="original-doc">
### Failed!

If you get a "Internal Server Error" or "No input file specified" error, try changing:

    RewriteRule ^(?:application|modules|system)\b - [F,L]

Instead, we can try a slash:

    RewriteRule ^(application|modules|system)/ - [F,L]

If that doesn't work, try changing:

    RewriteRule .* index.php/$0 [PT]

To something more simple:

    RewriteRule .* index.php [PT]
</div>

### まだ失敗！ {#still-failed}
まだエラーが起きるなら、あなたのホストが`mod_rewrite`をサポートしているか確認してください。Apacheの設定を変更することができるなら、これらの行を設定に加えてください。設定ファイルは通常`httpd.conf`です。

    <Directory "/var/www/html/myapp">
        Order allow,deny
        Allow from all
        AllowOverride All
    </Directory>

エラーを明確にするためにApacheのログもチェックするべきです。

<div class="original-doc">
### Still Failed!

If you are still getting errors, check to make sure that your host supports URL `mod_rewrite`. If you can change the Apache configuration, add these lines to the the configuration, usually `httpd.conf`:

    <Directory "/var/www/html/myapp">
        Order allow,deny
        Allow from all
        AllowOverride All
    </Directory>

You should also check your Apache logs to see if they can shed some light on the error.
</div>
## NGINX
Nginxの設定例を提示することは難しいのですが、これがサーバー設定のサンプルです。
<div class="original-doc">
It is hard to give examples of nginx configuration, but here is a sample for a server:
</div>

    location / {
        index     index.php index.html index.htm;
        try_files $uri index.php;
    }

    location = index.php {
        include       fastcgi.conf;
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_index index.php;
    }

もしこれを動作させるのに課題がある場合、nginxのデバッグレベルを有効にしてアクセスとエラーのログを確認してください。
<div class="original-doc">
If you are having issues getting this working, enable debug level logging in nginx and check the access and error logs.
</div>