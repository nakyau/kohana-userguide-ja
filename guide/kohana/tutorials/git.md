# 新しいアプリケーションの作成 {#creating-a-new-application}

[!!] 以下の例はあなたのウェブサーバが既にセットアップされてるものとして進めます。また、<http://localhost/gitorial/>にアプリケーションを作成するものとします。

コンソールから空のディレクトリ`gitorial`に移動し、`git init`を実行します。これは		gitの新しいbareリポジトリを作成します。

次に`system`ディレクトリの[サブモジュール](http://www.kernel.org/pub/software/scm/git/docs/git-submodule.html)を作成します。<http://github.com/kohana/core>に行って"Clone URL:"をコピーします。

<div class="original-doc">
# Creating a New Application

[!!] The following examples assume that your web server is already set up, and you are going to create a new application at <http://localhost/gitorial/>.

Using your console, change to the empty directory `gitorial` and run `git init`. This will create the bare structure for a new git repository.

Next, we will create a [submodule](http://www.kernel.org/pub/software/scm/git/docs/git-submodule.html) for the `system` directory. Go to <http://github.com/kohana/core> and copy the "Clone URL":
</div>

![Github Clone URL](http://img.skitch.com/20091019-rud5mmqbf776jwua6hx9nm1n.png)

今度は、そのURLを使って`system`のサブモジュールを作成します。

    git submodule add git://github.com/kohana/core.git system

[!!] これは現在の次の安定リリースの開発版へのリンクを作成します。開発版は使用に関してほとんど安全であるべきであり、バグフィックスが適用された現在の安定板と同じAPIを備えています。

必要なサブモジュールは何でも加えてください。[Database]モジュールが必要ならば:


<div class="original-doc">
Now use the URL to create the submodule for `system`:

    git submodule add git://github.com/kohana/core.git system

[!!] This will create a link to the current development version of the next stable release. The development version should almost always be safe to use, have the same API as the current stable download with bugfixes applied.

Now add whatever submodules you need. For example, if you need the [Database] module:
</div>

    git submodule add git://github.com/kohana/database.git modules/database

サブモジュールを追加したら初期化が必要です。
<div class="original-doc">
After submodules are added, they must be initialized:
</div>

    git submodule init

これでサブモジュールが追加され、それらをコミットすることが出来ます。
<div class="original-doc">
Now that the submodules are added, you can commit them:
</div>

    git commit -m 'Added initial submodules'

次に、アプリケーションにディレクトリ構造を作成してください。これは要求される最小の構成です。
<div class="original-doc">
Next, create the application directory structure. This is the bare minimum required:
</div>

    mkdir -p application/classes/{controller,model}
    mkdir -p application/{config,views}
    mkdir -m 0777 -p application/{cache,logs}

`find application`を実行すれば、以下のように表示されるはずです。
<div class="original-doc">
If you run `find application` you should see this:
</div>

    application
    application/cache
    application/config
    application/classes
    application/classes/controller
    application/classes/model
    application/logs
    application/views

logsやcacheはgitで記録したくありません。ですから、各ディレクトリに`.gitignore`ファイルを追加します。これは全ての非隠しファイルを無視します。
<div class="original-doc">
We don't want git to track log or cache files, so add a `.gitignore` file to each of the directories. This will ignore all non-hidden files:
</div>
    echo '[^.]*' > application/{logs,cache}/.gitignore


[!!] Gitは空のディレクトリを無視します。従って`.gitignore`ファイルを加えることはGitがそのディレクトリ中のファイルではなくディレクトリを記録することを確実にします。

<div class="original-doc">
[!!] Git ignores empty directories, so adding a `.gitignore` file also makes sure that git will track the directory, but not the files within it.

Now we need the `index.php` and `bootstrap.php` files:
</div>

    wget https://github.com/kohana/kohana/raw/3.1/master/index.php --no-check-certificate
    wget https://github.com/kohana/kohana/raw/3.1/master/application/bootstrap.php --no-check-certificate -O application/bootstrap.php

これらの変更もコミットしてください。
<div class="original-doc">
Commit these changes too:
</div>

    git add application
    git commit -m 'Added initial directory structure'

これで全部です。バージョン管理にGitを使用するアプリケーションが出来ました。
<div class="original-doc">
That's all there is to it. You now have an application that is using Git for versioning.
</div>


## サブモジュールの追加 {#adding-submodules}
新しいサブモジュールを追加するには以下の手順を行います。

1. レポジトリのパスから新しいgitサブモジュールを追加するコードを実行してください。例:

        git submodule add git://github.com/shadowhand/sprig.git modules/sprig

2. 次にサブモジュールのinitとupdateを行います:

        git submodule init
        git submodule update

<div class="original-doc">
## Adding Submodules
To add a new submodule complete the following steps:

1. run the following code - git submodule add repository path for each new submodule e.g.:

        git submodule add git://github.com/shadowhand/sprig.git modules/sprig

2. then init and update the submodules:

        git submodule init
        git submodule update
</div>

## サブモジュールの更新 {#updating-submodules}

いつかあなたはサブモジュールをアップデートしたいと思うことがあるでしょう。すべてのサブモジュールを最新の`HEAD`バージョンに更新するには:

    git submodule foreach 'git checkout 3.1/master && git pull origin 3.1/master'

単独のサブモジュールを更新する場合、例えば`system`ならば:

    cd system
    git checkout 3.1/master
    git pull origin 3.1/master
    cd ..
    git add system
    git commit -m 'Updated system to latest version'

1つのサブモジュールを特定のコミットにアップデートしたいならば:

    cd modules/database
    git pull origin 3.1/master
    git checkout fbfdea919028b951c23c3d99d2bc1f5bbeda0c0b
    cd ../..
    git add database
    git commit -m 'Updated database module'

公式のリリースポイントとしてタグ付けされたコミットをチェックアウトできることにも注目してください。例えば:

    git checkout 3.1.0

全てのタグの一覧を取得するには、単に`git tag`を引数なしで実行します。


<div class="original-doc">
## Updating Submodules

At some point you will probably also want to upgrade your submodules. To update all of your submodules to the latest `HEAD` version:

    git submodule foreach 'git checkout 3.1/master && git pull origin 3.1/master'

To update a single submodule, for example, `system`:

    cd system
    git checkout 3.1/master
    git pull origin 3.1/master
    cd ..
    git add system
    git commit -m 'Updated system to latest version'

If you want to update a single submodule to a specific commit:

    cd modules/database
    git pull origin 3.1/master
    git checkout fbfdea919028b951c23c3d99d2bc1f5bbeda0c0b
    cd ../..
    git add database
    git commit -m 'Updated database module'

Note that you can also check out the commit at a tagged official release point, for example:

    git checkout 3.1.0

Simply run `git tag` without arguments to get a list of all tags.
</div>

## サブモジュールの削除 {#removing-submodules}
必要ではなくなったサブモジュールを削除するには、次の手順を行います。

1. .gitmodulesを開き、サブモジュールへの参照を削除する。
　それはこのように表示されているでしょう:

        [submodule "modules/auth"]
        path = modules/auth
        url = git://github.com/kohana/auth.git

2. .git/configファイルを開き、サブモジュールへの参照を削除する

        [submodule "modules/auth"]
        url = git://github.com/kohana/auth.git

3. git rm --cached path/to/submodule を実行する。例:

        git rm --cached modules/auth

**注意:** パスの最後にスラッシュを付けないでください。もしパスの最後にスラッシュを付ければ失敗します。

<div class="original-doc">
## Removing Submodules
To remove a submodule that is no longer needed complete the following steps:

1. open .gitmodules and remove the reference to the to submodule
    It will look something like this:

        [submodule "modules/auth"]
        path = modules/auth
        url = git://github.com/kohana/auth.git

2. open .git/config and remove the reference to the to submodule\\

        [submodule "modules/auth"]
        url = git://github.com/kohana/auth.git

3. run git rm --cached path/to/submodule, e.g.

        git rm --cached modules/auth

**Note:** Do not put a trailing slash at the end of path. If you put a trailing slash at the end of the command, it will fail.
</div>


## リモートリポジトリURLの更新 {#updating-remote-repository-url}

プロジェクトの開発中に、何らかの理由でサブモジュールのソースが変更（あなたが新しいフォームを作る、サーバーのURLが変わる、リポジトリの名前やパスが変わる等）になって、それらの変更を更新しなくてはなりません。更新するには以下の手順を行います。


1. .gitmodules ファイルを開き, 変更されたサブモジュールのURLを変更する。

2. ソースツリーのルートに置いて 次のコマンドを実行する:

		git submodule sync

3. 新しいURLを設定したプロジェクトのリポジトリを更新するために`git init`を実行する:

		git submodule init

これで終わりです。これでサブモジュールのpushとpullを問題なく続けることが出来ます。

<div class="original-doc">
## Updating Remote Repository URL

During the development of a project, the source of a submodule may change for any reason (you've created your own fork, the server URL changed, the repository name or path changed, etc...) and you'll have to update those changes. To do so, you'll need to perform the following steps:

1. edit the .gitmodules file, and change the URL for the submodules which changed.

2. in your source tree's root run:

		git submodule sync

3. run `git init` to update the project's repository configuration with the new URLs:

		git submodule init

And it's done, now you can continue pushing and pulling your submodules with no problems.
</div>
Source: http://jtrancas.wordpress.com/2011/02/06/git-submodule-location/