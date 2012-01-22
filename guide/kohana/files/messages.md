# メッセージ {#messages}

システムメッセージを定義できるように、キーに基づいた強力な検索システムをKohanaは備えています。
<div class="original-doc">
# Messages

Kohana has a robust key based lookup system so you can define system messages.
</div>
## メッセージを取得する {#getting-a-message}

Kohana::message()メソッドを使ってメッセージキーを取得します:

	Kohana::message('forms', 'foobar');

このコードは`messages/forms.php`ファイルを`foobar`キーで検索します。

<div class="original-doc">
## Getting a message

Use the Kohana::message() method to get a message key:

	Kohana::message('forms', 'foobar');

This will look in the `messages/forms.php` file for the `foobar` key:

	<?php
	
	return array(
		'foobar' => 'Hello, world!',
	);
</div>

サブフォルダやサブキーを検索することもできます:

	Kohana::message('forms/contact', 'foobar.bar');

これは`messages/forms/contact.php`の中で`[foobar][bar]`を検索します:

<div class="original-doc">
You can also look in subfolders and sub-keys:

	Kohana::message('forms/contact', 'foobar.bar');

This will look in the `messages/forms/contact.php` for the `[foobar][bar]` key:
</div>
	<?php
	
	return array(
		'foobar' => array(
			'bar' => 'Hello, world!',
		),
	);

## 注意 {#notes}

 * これらのファイルはキャッシュされ、おそらくは正しく動作しないのでメッセージファイル中で__() 使わないでください。
 * メッセージはカスケーディングファイルシステムによってマージされます。クラスやビューの様に上書きはされません。
<div class="original-doc">
## Notes

 * Don't use __() in your messages files, as these files can be cached and will not work properly.
 * Messages are merged by the cascading file system, not overwritten like classes and views.
</div>