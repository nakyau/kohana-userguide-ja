# クロスサイトスクリプティング(XSS) {#cross-site-scripting-xss-security}
* このページは包括的ではなく、XSS防止の完全なガイドではありません。*
[XSS](http://wikipedia.org/wiki/Cross-Site_Scripting)攻撃を防止する第一歩は、いつ防御が必要かを知ることです。
XSSはフォームの入力内容やデータベースの検索結果などをHTMLコンテンツが表示されるときにだけ起こります。
`$_GET`,`$_POST`と`$_COOKIES`を含むユーザから得た全ての情報は汚染されることがあります。

<div class="original-doc">
# Cross-Site Scripting (XSS) Security

*This page is not comprehensive and should not be considered a complete guide to XSS prevention.*

The first step to preventing [XSS](http://wikipedia.org/wiki/Cross-Site_Scripting) attacks is knowing when you need to protect yourself. XSS can only be triggered when it is displayed within HTML content, sometimes via a form input or being displayed from database results. Any global variable that contains client information can be tainted. This includes `$_GET`, `$_POST`, and `$_COOKIE` data.
</div>

## 予防 {#preventation}
XSSからあなたのアプリケーションを保護するために従うべき、いくつかのルールがあります。
変数のHTML化を望まないのであれば、不要なHTMLタグを取り除くために[strip_tags](http://php.net/strip_tags)を使用してください。

[!!] ユーザにHTML化される入力を許可する場合は [HTML Purifier](http://htmlpurifier.org/) や [HTML Tidy](http://php.net/tidy) のようなHTMLクリーニングツールを使用することを強くお勧めします。
<div class="original-doc">
## Prevention

There are a few simple rules to follow to guard your application HTML against XSS. If you do not want HTML in a variable, use [strip_tags](http://php.net/strip_tags) to remove all unwanted HTML tags from a value.

[!!] If you allow users to submit HTML to your application, it is highly recommended to use an HTML cleaning tool such as [HTML Purifier](http://htmlpurifier.org/) or [HTML Tidy](http://php.net/tidy).

The second is to always escape data when inserting into HTML. The [HTML] class provides generators for many common tags, including script and stylesheet links, anchors, images, and email (mailto) links. Any untrusted content should be escaped using [HTML::chars].
</div>

## 参考 {#references}

* [OWASP XSSチートシート](http://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet)

<div class="original-doc">
## References

* [OWASP XSS Cheat Sheet](http://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet)
</div>