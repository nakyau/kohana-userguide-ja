# ヘルパー

Kohanaには仕事をより簡単にするためのスタティックなヘルパー機能が付属します。
クラスを作成し、`classes`ディレクトリに置くだけで、あなた独自のヘルパーを作ることができます。
さらに透過的拡張を使用し、機能を変更したり追加してヘルパーを拡張できます。

 - **[Arr]** - 配列関数。 配列のキーを取得、デフォルト値の設定、パスによる配列キーの取得など。

 - **[CLI]** - コマンドラインオプションをパースする

 - **[Cookie]** - 詳細は [Cookies](cookies) ページ

 - **[Date]** - 便利な日付関数と定数。2つの日付間の時間、AM/PMの変換、日付のオフセットなど。
 
 - **[Encrypt]** - 詳細は [Security](security) ページ
 
 - **[Feed]** - RSSフィードのパースと作成
 
 - **[File]** - MIMEからのファイルタイプ取得、ファイルの分割と統合
 
 - **[Form]** - HTMLフォーム要素の作成 
 
 - **[Fragment]** - シンプルなファイルベースのキャッシュ。詳細は[Fragments](fragments) ページ

 - **[HTML]** - 便利なHTMLの機能。 エンコード、難読化、スクリプト作成、アンカー、imgタグなど。
 
 - **[I18n]** - 他言語サイトのための国際化ヘルパ。
 
 - **[Inflector]** - 単語の単数系／複数形変換、句のキャメル化、文章化
 
 - **[Kohana]** - Kohanaクラスもヘルパーです。(print_rに似てより良い)変数のデバッグ、ファイル読み込みなど。
 
 - **[Num]** - ロケールに即した書式と英語の除数(th, st, nd, など）を提供。 
 
 - **[Profiler]** - 詳細は[Profiling](profiling) ページ

 - **[Remote]** - [CURL](http://php.net/curl)を使用したリモートサーバアクセスヘルパー。
 
 - **[Request]** - 現在のリクエストURLの取得、タグ作成、ファイル送信、ユーザエージェントの取得など。 
 
 - **[Route]** - ルートの作成、ルートを使用した内部リンクの作成。
 
 - **[Security]** - 詳細は[Security](security) ページ。
 
 - **[Session]** - 詳細は[Sessions](sessions) ページ。
 
 - **[Text]** - 自動リンク、エスケープ、数値からテキストへの変換
 
 - **[URL]** - 相対パス／絶対パスの作成、URLセーフなタイトル作成など。
 
 - **[UTF8]** - strlen, strpos, substrのようなマルチバイト文字列関数を提供。
 
 - **[Upload]** - フォームからファイルをアップロードするためのヘルパー。
<div class="original-doc">
# Helpers

Kohana comes with many static "helper" functions to make certain tasks easier.

You can make your own helpers by simply making a class and putting it in the `classes` directory, and you can also extend any helper to modify or add new functions using transparent extension.

 - **[Arr]** - Array functions.  Get an array key or default to a set value, get an array key by path, etc.

 - **[CLI]** - Parse command line options.

 - **[Cookie]** - Covered in more detail on the [Cookies](cookies) page.

 - **[Date]** - Useful date functions and constants. Time between two dates, convert between am/pm and military, date offset, etc.
 
 - **[Encrypt]** - Covered in more detail on the [Security](security) page.
 
 - **[Feed]** - Parse and create RSS feeds.
 
 - **[File]** - Get file type by mime, split and merge a file into small pieces.
 
 - **[Form]** - Create HTML form elements. 
 
 - **[Fragment]** - Simple file based caching. Covered in more detail on the [Fragments](fragments) page.

 - **[HTML]** - Useful HTML functions. Encode, obfuscate, create script, anchor, and image tags, etc.
 
 - **[I18n]** - Internationalization helper for creating multilanguage sites.
 
 - **[Inflector]** - Change a word into plural or singular form, camelize or humanize a phrase, etc.
 
 - **[Kohana]** - The Kohana class is also a helper.  Debug variables (like print_r but better), file loading, etc.
 
 - **[Num]** - Provides locale aware formating and english ordinals (th, st, nd, etc).
 
 - **[Profiler]** - Covered in more detail on the [Profiling](profiling) page.

 - **[Remote]** - Remote server access helper using [CURL](http://php.net/curl).
 
 - **[Request]** - Get the current request url, create expire tags, send a file, get the user agent, etc. 
 
 - **[Route]** - Create routes, create an internal link using a route.
 
 - **[Security]** - Covered in more detail on the [Security](security) page.
 
 - **[Session]** - Covered in more detail on the [Sessions](sessions) page.
 
 - **[Text]** - Autolink, prevent window words, convert a number to text, etc.
 
 - **[URL]** - Create a relative or absolute URL, make a URL-safe title, etc.
 
 - **[UTF8]** - Provides multi-byte aware string functions like strlen, strpos, substr, etc.
 
 - **[Upload]** - Helper for uploading files from a form.
</div>