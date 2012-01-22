# 規約とコーディングスタイル {#conventions-and-coding-styles}

Kohanaのコーディングスタイルに倣うことを推奨します。このコーディングスタイルを採用することでコードを読みやすくし、コードの共有や提供を容易にします。

<div class="original-doc">
# Conventions and Coding Style

It is encouraged that you follow Kohana's coding style. This makes code more readable and allows for easier code sharing and contributing. 
</div>
## クラス名とファイルの配置場所 {#class-names-and-file-location}

Kohanaのクラス名は[autoloading](autoloading)を容易にするために厳格な規約に従っています。
クラス名は１文字目が大文字でワードをアンダースコアで区切ったものでなければなりません。アンダースコアはファイルが配置されているディレクトリを意味しています。
以下の規約が適用されます:

1. 意図しないディレクトリが作成されることがあるので、キャメルケースの名前は使うべきではありません。
2. すべてのクラスファイルとディレクトリの名前は小文字とする。
3. クラスファイルはすべてclassesディレクトリに配置する。これによりどのようなレベルのカスケードィングファイルシステムをも利用可能にします。

<div class="original-doc">
## Class Names and File Location

Class names in Kohana follow a strict convention to facilitate [autoloading](autoloading). Class names should have uppercase first letters with underscores to separate words. Underscores are significant as they directly reflect the file location in the filesystem.

The following conventions apply:

1. CamelCased class names should not be used, except when it is undesirable to create a new directory level.
2. All class file names and directory names are lowercase.
3. All classes should be in the `classes` directory. This may be at any level in the [cascading filesystem](files).
</div>
### 例  {#class-name-examples}
クラスにおいてアンダースコアはディレクトリを意味することを覚えておいてください。
以下の例を確認してください:

Class Name            | File Path
----------------------|-------------------------------
Controller_Template   | classes/controller/template.php
Model_User            | classes/model/user.php
Database              | classes/database.php
Database_Query        | classes/database/query.php
Form                  | classes/form.php

<div class="original-doc">
### Examples  {#class-name-examples}

Remember that in a class, an underscore means a new directory. Consider the following examples:

Class Name            | File Path
----------------------|-------------------------------
Controller_Template   | classes/controller/template.php
Model_User            | classes/model/user.php
Database              | classes/database.php
Database_Query        | classes/database/query.php
Form                  | classes/form.php
</div>
## コーディング標準 {#coding-stanstandards}

一貫したコードを作成するためにコーディング標準に可能な限り厳密に従うことを、我々は皆さんにお願いします。

<div class="original-doc">
## Coding Standards

In order to produce highly consistent source code, we ask that everyone follow the coding standards as closely as possible.
</div>
### 括弧 {#brackets}

[BSD/Allman スタイル](http://en.wikipedia.org/wiki/Indent_style#BSD.2FAllman_style) の括弧を使ってください。  

<div class="original-doc">
### Brackets

Please use [BSD/Allman Style](http://en.wikipedia.org/wiki/Indent_style#BSD.2FAllman_style) bracketing.  
</div>
#### 波括弧 {#curly-brackets}

波括弧は括弧だけで１行を使い、制御文を同じレベルに見せます。
	// 正しい
	if ($a === $b)
	{
		...
	}
	else
	{
		...
	}

	// 間違い
	if ($a === $b) {
		...
	} else {
		...
	}

<div class="original-doc">
#### Curly Brackets

Curly brackets are placed on their own line, indented to the same level as the control statement.

	// Correct
	if ($a === $b)
	{
		...
	}
	else
	{
		...
	}

	// Incorrect
	if ($a === $b) {
		...
	} else {
		...
	}
</div>

#### クラスの括弧 {#class-brackets}

波括弧の規則の唯一の例外は、クラスの開始の括弧はクラス名と同じ行に記述することです。

	// 正しい
	class Foo {

	// 間違い
	class Foo
	{

<div class="original-doc">
#### Class Brackets

The only exception to the curly bracket rule is, the opening bracket of a class goes on the same line.

	// Correct
	class Foo {

	// Incorrect
	class Foo
	{
</div>
#### 空の括弧 {#empty-brackets}

空の括弧の中には、どんな文字も入れないでください。

	// 正しい
	class Foo {}

	// 間違い
	class Foo { }

<div class="original-doc">
#### Empty Brackets

Don't put any characters inside empty brackets.

	// Correct
	class Foo {}

	// Incorrect
	class Foo { }
</div>

#### 配列の括弧 {#array-brackets}

配列は1行に書くこともでも複数行に書くこともできます。

	array('a' => 'b', 'c' => 'd')
	
	array(
		'a' => 'b', 
		'c' => 'd',
	)

<div class="original-doc">
#### Array Brackets

Arrays may be single line or multi-line.

	array('a' => 'b', 'c' => 'd')
	
	array(
		'a' => 'b', 
		'c' => 'd',
	)
</div>

##### 開始の丸括弧 {#opeing-brackets}

配列の開始を表す丸括弧はarrayと同じ行に置きます。

	// 正しい
	array(
		...
	)

	// 間違い
	array
	(
		...
	)

<div class="original-doc">
##### Opening Parenthesis

The opening array parenthesis goes on the same line.

	// Correct
	array(
		...
	)

	// Incorrect:
	array
	(
		...
	)
</div>
##### 終了の丸括弧 {#closing-brackets}

###### 1次元配列 {#single-dimension}

複数行にわたる1次元配列を閉じる丸括弧は、同レベルの値と制御文をそれぞれ表すために、括弧1つで1行を使います。

	// 正しい
	$array = array(
		...
	)

	// 間違い
	$array = array(
		...
	)

<div class="original-doc">
##### Closing parenthesis

###### Single Dimension

The closing parenthesis of a multi-line single dimension array is placed on its own line, indented to the same level as the assignment or statement.

	// Correct
	$array = array(
		...
	)

	// Incorrect
	$array = array(
		...
		)
</div>
###### 多次元配列 {#multidimensional}

ネストされた配列は1次元配列のルールに従って、1つのタブでインデントされます。


	// 正しい
	array(
		'arr' => array(
			...
		),
		'arr' => array(
			...
		),
	)
	
	array(
		'arr' => array(...),
		'arr' => array(...),
	)
	
<div class="original-doc">
###### Multidimensional

The nested array is indented one tab to the right, following the single dimension rules.

	// Correct
	array(
		'arr' => array(
			...
		),
		'arr' => array(
			...
		),
	)
	
	array(
		'arr' => array(...),
		'arr' => array(...),
	)
</div>
##### 関数の引数としての配列 {#array-as-function-argument}


	// 正しい
	do(array(
		...
	))
	
	// 間違い
	do(array(
		...
	))

<div class="original-doc">	
##### Arrays as Function Arguments


	// 正しい
	do(array(
		...
	))
	
	// 間違い
	do(array(
		...
		))

As noted at the start of the array bracket section, single line syntax is also valid.

	// Correct
	do(array(...))
	
	// Alternative for wrapping long lines
	do($bar, 'this is a very long line',
		array(...));
</div>

### 命名規則 {#naming-convention}

Kohanaはキャメルケースではなく、アンダースコアを持つ名前を使います。

<div class="original-doc">
### Naming Conventions

Kohana uses under_score naming, not camelCase naming.
</div>
#### クラス {#classes}

	// Controller クラス、 Controller_ プレフィックスを使う
	class Controller_Apple extends Controller {

	// Model クラス、 Model_ プレフィックスを使う
	class Model_Cheese extends Model {

	// 通常のクラス
	class Peanut {

クラスのインスタンスを作成するとき、コンストラクタに値を渡さない場合は丸括弧を使わないでください。

	// 正しい:
	$db = new Database;

	// 間違い:
	$db = new Database();

<div class="original-doc">
#### Classes

	// Controller class, uses Controller_ prefix
	class Controller_Apple extends Controller {

	// Model class, uses Model_ prefix
	class Model_Cheese extends Model {

	// Regular class
	class Peanut {

When creating an instance of a class, don't use parentheses if you're not passing something on to the constructor:

	// Correct:
	$db = new Database;

	// Incorrect:
	$db = new Database();
</div>

#### 関数とメソッド {#functions-and-methods}

関数名は全て小文字からなります。語を区切るのにアンダースコアを使用します：

<div class="original-doc">
#### Functions and Methods

Functions should be all lowercase, and use under_scores to separate words:
</div>
	function drink_beverage($beverage)
	{

#### 変数 {#variables}

すべての変数はキャメルではなく、小文字とアンダースコアを使います:

	// 正しい
	$foo = 'bar';
	$long_example = 'uses underscores';

	// 間違い
	$weDontWantThis = 'understood?';

<div class="original-doc">
#### Variables

All variables should be lowercase and use under_score, not camelCase:

	// Correct:
	$foo = 'bar';
	$long_example = 'uses underscores';

	// Incorrect:
	$weDontWantThis = 'understood?';
</div>
### インデント {#indentation}

コードのインデントにはタブを使わなくてはいけません。
タブにスペースを使うことは厳密に禁じられています。

垂直方向の余白（複数行にわたる記述用）はスペースで行ないます。タブは人によって幅が異なるので、垂直をあわせるのには向いていません。

<div class="original-doc">
### Indentation

You must use tabs to indent your code. Using spaces for tabbing is strictly forbidden.

Vertical spacing (for multi-line) is done with spaces. Tabs are not good for vertical alignment because different people have different tab widths.
</div>

	$text = 'this is a long text block that is wrapped. Normally, we aim for '
		  .'wrapping at 80 chars. Vertical alignment is very important for '
		  .'code readability. Remember that all indentation is done with tabs,'
		  .'but vertical alignment should be completed with spaces, after '
		  .'indenting with tabs.';

### 文字列の結合 {#string-concatnation}

連結子の周りにスペースを置かないでください:

	// 正しい
	$str = 'one'.$var.'two';

	// 間違い
	$str = 'one'. $var .'two';
	$str = 'one' . $var . 'two';

<div class="original-doc">
### String Concatenation

Do not put spaces around the concatenation operator:

	// Correct:
	$str = 'one'.$var.'two';

	// Incorrect:
	$str = 'one'. $var .'two';
	$str = 'one' . $var . 'two';

</div>

### 単行構文 {#single-line-statements}

単行のIF文は実行をブレークするときだけに使われるべきです。（例えばreturnやcontinue）

	// 許容される
	if ($foo == $bar)
		return $foo;

	if ($foo == $bar)
		continue;

	if ($foo == $bar)
		break;

	if ($foo == $bar)
		throw new Exception('You screwed up!');

	// 許容されない
	if ($baz == $bun)
		$baz = $bar + 2;

<div class="original-doc">
### Single Line Statements

Single-line IF statements should only be used when breaking normal execution (e.g. return or continue):

	// Acceptable:
	if ($foo == $bar)
		return $foo;

	if ($foo == $bar)
		continue;

	if ($foo == $bar)
		break;

	if ($foo == $bar)
		throw new Exception('You screwed up!');

	// Not acceptable:
	if ($baz == $bun)
		$baz = $bar + 2;
</div>

### 比較演算子 {#conparision-operations}

比較演算子はORとANDを使用してください:

	// 正しい
	if (($foo AND $bar) OR ($b AND $c))

	// 間違い
	if (($foo && $bar) || ($b && $c))
	
else ifではなく、elseifを使用してください:

	// 正しい
	elseif ($bar)

	// 間違い
	else if($bar)

<div class="original-doc">
### Comparison Operations

Please use OR and AND for comparison:

	// Correct:
	if (($foo AND $bar) OR ($b AND $c))

	// Incorrect:
	if (($foo && $bar) || ($b && $c))
	
Please use elseif, not else if:

	// Correct:
	elseif ($bar)

	// Incorrect:
	else if($bar)
</div>

### switch構文 {#switch-structures}

case,break,defaultはそれぞれ別の行に記述します。caseブロック、defaultブロックの中はタブ１個でインデントします。
<div class="original-doc">
### Switch Structures

Each case, break and default should be on a separate line. The block inside a case or default must be indented by 1 tab.
</div>

	switch ($var)
	{
		case 'bar':
		case 'foo':
			echo 'hello';
		break;
		case 1:
			echo 'one';
		break;
		default:
			echo 'bye';
		break;
	}

### 丸括弧 {#parentheses}

文のあとには１つのスペースを置き、括弧が続きます。!文字は読みやすいように前後にスペースを空けます。!や型キャストの場合以外には開始括弧のあと、終了括弧の前にスペースは置きません。

	// 正しい
	if ($foo == $bar)
	if ( ! $foo)

	// 間違い
	if($foo == $bar)
	if(!$foo)
	if ((int) $foo)
	if ( $foo == $bar )
	if (! $foo)

<div class="original-doc">
### Parentheses

There should be one space after statement name, followed by a parenthesis. The ! (bang) character must have a space on either side to ensure maximum readability. Except in the case of a bang or type casting, there should be no whitespace after an opening parenthesis or before a closing parenthesis.

	// Correct:
	if ($foo == $bar)
	if ( ! $foo)

	// Incorrect:
	if($foo == $bar)
	if(!$foo)
	if ((int) $foo)
	if ( $foo == $bar )
	if (! $foo)

</div>

### 3項演算 {#ternaries}

すべての３項演算は標準書式に従います。丸括弧は式にのみ使い、値には使用しません。

	$foo = ($bar == $foo) ? $foo : $bar;
	$foo = $bar ? $foo : $bar;

すべての比較と処理は括弧の中で完了しなければなりません:

	$foo = ($bar > 5) ? ($bar + $foo) : strlen($bar);

複雑な参考演算（最初の部分が80文字を越えるような）は行をわけて書き、スペースで演算子をそろえます:

	$foo = ($bar == $foo)
		 ? $foo
		 : $bar;

<div class="original-doc">
### Ternaries

All ternary operations should follow a standard format. Use parentheses around expressions only, not around just variables.

	$foo = ($bar == $foo) ? $foo : $bar;
	$foo = $bar ? $foo : $bar;

All comparisons and operations must be done inside of a parentheses group:

	$foo = ($bar > 5) ? ($bar + $foo) : strlen($bar);

When separating complex ternaries (ternaries where the first part goes beyond ~80 chars) into multiple lines, spaces should be used to line up operators, which should be at the front of the successive lines:

	$foo = ($bar == $foo)
		 ? $foo
		 : $bar;
</div>

### 型変換 {#type-casting}

型変換はキャストの両側にスペースを置きます:

	// 正しい
	$foo = (string) $bar;
	if ( (string) $bar)

	// 間違い
	$foo = (string)$bar;

可能であれば、3項演算の代わりに型変換を使用してください。:

	// 正しい
	$foo = (bool) $bar;

	// 間違い
	$foo = ($bar == TRUE) ? TRUE : FALSE;

整数またはブーリアンに型変換する場合は、短い書式を使います。

	// 正しい
	$foo = (int) $bar;
	$foo = (bool) $bar;

	// 間違い
	$foo = (integer) $bar;
	$foo = (boolean) $bar;

<div class="original-doc">
### Type Casting

Type casting should be done with spaces on each side of the cast:

	// Correct:
	$foo = (string) $bar;
	if ( (string) $bar)

	// Incorrect:
	$foo = (string)$bar;

When possible, please use type casting instead of ternary operations:

	// Correct:
	$foo = (bool) $bar;

	// Incorrect:
	$foo = ($bar == TRUE) ? TRUE : FALSE;

When casting type to integer or boolean, use the short format:

	// Correct:
	$foo = (int) $bar;
	$foo = (bool) $bar;

	// Incorrect:
	$foo = (integer) $bar;
	$foo = (boolean) $bar;
</div>

### 定数 {#constants}

定数にはいつでも大文字を使用します:

	// 正しい
	define('MY_CONSTANT', 'my_value');
	$a = TRUE;
	$b = NULL;

	// 間違い
	define('MyConstant', 'my_value');
	$a = True;
	$b = null;

定数との評価をするときは右辺に定数を置きます:

	// 正しい
	if ($foo !== FALSE)

	// 間違い
	if (FALSE !== $foo)

これは少々口うるさい選択ですので、理由を説明します。
もし我々が上記の例を英語で書いた場合、正しい例はこう読めるでしょう:

	if variable $foo is not exactly FALSE

そして、誤った例はこう読めます:

	if FALSE is not exactly variable $foo

我々は左から右に読むので、定数を最初に持ってくると単に意味をなさなくなります。


<div class="original-doc">
### Constants

Always use uppercase for constants:

	// Correct:
	define('MY_CONSTANT', 'my_value');
	$a = TRUE;
	$b = NULL;

	// Incorrect:
	define('MyConstant', 'my_value');
	$a = True;
	$b = null;

Place constant comparisons at the end of tests:

	// Correct:
	if ($foo !== FALSE)

	// Incorrect:
	if (FALSE !== $foo)

This is a slightly controversial choice, so I will explain the reasoning. If we were to write the previous example in plain English, the correct example would read:

	if variable $foo is not exactly FALSE

And the incorrect example would read:

	if FALSE is not exactly variable $foo

Since we are reading left to right, it simply doesn't make sense to put the constant first.
</div>

### コメント {#comments}

#### 単行コメント {#one-line-comments}

コメントをつけるコードの上に//を｢使ってコメントします。//の後ろにスペース１つ置き、大文字で始めます。#は決して使わないでください。

	// Correct 正しい

	//Incorrect 間違い
	// incorrect 間違い
	# Incorrect 間違い

<div class="original-doc">
### Comments

#### One-line Comments

Use //, preferably above the line of code you're commenting on. Leave a space after it and start with a capital. Never use #.

	// Correct

	//Incorrect
	// incorrect
	# Incorrect
</div>

### 正規表現 {#regular-expressions}

正規表現を書くときはPOSIXよりもPCREを使用してください。PCREは強力で高速です。

	// 正しい
	if (preg_match('/abc/i', $str))

	// 間違い
	if (eregi('abc', $str))

正規表現の前後にはダブルクォートよりもシングルクォートを優先して使います。シングルクォートは単純であるために便利に使用できます。ダブルクォートと違って、シングルクォートは\nや\tのようなバックスラッシュによる制御文字の入力をサポートしていません。

	// 正しい
	preg_match('/abc/', $str);

	// 間違い
	preg_match("/abc/", $str);

正規表現で検索や置き換えをプログラミングする場合は後方参照に$nを使ってください。こちらのほうが\nより好まれます。

	// 正しい
	preg_replace('/(\d+) dollar/', '$1 euro', $str);

	// 間違い
	preg_replace('/(\d+) dollar/', '\\1 euro', $str);

最後に、行末にマッチする$文字は改行文字（\n）を許可することに注意してください。行末だけにマッチする必要があれば、D修飾子を使います。

	$str = "email@example.com\n";

	preg_match('/^.+@.+$/', $str);  // TRUE
	preg_match('/^.+@.+$/D', $str); // FALSE
<div class="original-doc">
### Regular Expressions

When coding regular expressions please use PCRE rather than the POSIX flavor. PCRE is considered more powerful and faster.

	// Correct:
	if (preg_match('/abc/i', $str))

	// Incorrect:
	if (eregi('abc', $str))

Use single quotes around your regular expressions rather than double quotes. Single-quoted strings are more convenient because of their simplicity. Unlike double-quoted strings they don't support variable interpolation nor integrated backslash sequences like \n or \t, etc.

	// Correct:
	preg_match('/abc/', $str);

	// Incorrect:
	preg_match("/abc/", $str);

When performing a regular expression search and replace, please use the $n notation for backreferences. This is preferred over \\n.

	// Correct:
	preg_replace('/(\d+) dollar/', '$1 euro', $str);

	// Incorrect:
	preg_replace('/(\d+) dollar/', '\\1 euro', $str);

Finally, please note that the $ character for matching the position at the end of the line allows for a following newline character. Use the D modifier to fix this if needed. [More info](http://blog.php-security.org/archives/76-Holes-in-most-preg_match-filters.html).

	$str = "email@example.com\n";

	preg_match('/^.+@.+$/', $str);  // TRUE
	preg_match('/^.+@.+$/D', $str); // FALSE
</div>
