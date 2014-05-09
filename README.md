楽しくコーディング！Sass,Compassラのデフォルトmininxとカスタム関数
フォルダ構成
---
	├config.rb
	├	rb
	|	├ develo.rb
	|	├ skyward_design.rb
	|	└
	└	sass
		├ _animation.scss
		├ _keyframes.sass
		├ _extension_decimal.scss
		├ _extension_import.scss
		├ _extension_sprite.scss
		├ _extension.scss
		├ _setting_site.scss
		├ _setting.scss
		├ common.scss
		├ default.scss
		├ import.scss
		├ sprite.scss
		├ templates.scss
		└ test.scss



全てのscssに共通で読み込む必要なmixinなどのimport（_extension_import.scss）
---
	@import "compass/css3/",
	"setting",
	"setting_site",
	"compass/support",
	"animation",
	"_keyframes",
	"extension_decimal",
	"extension";


[MIT]: http://terkel.jp/archives/2012/12/decimal-digits-and-rounding-sass-function/


animation関係を使えるように（_animation.scss）
---
下記のanimation関係のCSSを@includeで使えるようにしベンダープレフィックスを付けるようにしてくれる。
* animation-name
* animation-duration
* animation-timing-function
* animation-delay
* animation-iteration-count
* animation-direction


keyframesを使えるように（_keyframes.scss）
---
keyframesを@includeで使えるようにしベンダープレフィックスを付けるようにしてくれる。
		
	@include keyframes(anime1) {
	  0% {
	    @include transform(rotate(720deg));
	  }    
	  100% {
	  	width: 500px;
	    @include transform(rotate(0deg));
	  }
	}

小数の桁数と丸め方を制御する（_extension_decimal.scss）
---
下記サイトのscssを使わせて頂いています。
* [小数の桁数と丸め方を制御する Sass 関数][mit]

[MIT]: http://terkel.jp/archives/2012/12/decimal-digits-and-rounding-sass-function/


便利なmixin（_extension.scss）
---

### background-fit

画像のサイズを取得しbackground-size,width,heightをセットする。
CSSで背景に画像を入れる時に毎回サイズを入れる作業を省いてくれます。特にRetina対応で画像サイズを半分にしたいときには便利です。

#### 使い方
1. まず前提として、config.rbの<code>images_dir</code>を設定してください。
2. width,heightが必要ない場合には第2引き数を<code>false</code>にする。
3. 複数画像を指定したい場合は配列で（例: <code>('test.png','test2.gif',linear-gradient(#000,#fff)</code>）
4. 複数画像を指定した場合は第3引き数に複数指定する。（例: <code>('0 0 no-repeat','left top repeat','0 0 no-repeat')</code>）
5. 関数実行後$imgWidth,$imgHeightの中に画像のサイズが入ります。<br>複数の場合配列に入るのでnth($imgWidth,1),nth($imgHeight,2)のように取ってくることができます。

##### 説明
	@include background-fit(
		画像のパス, //images_dirで指定したパスからの画像ファイル名（配列で複数指定可能,グラデーション指定も可能）
		true or false, //（画像をサイズいっぱいに表示してテキストを消す or 画像とbackgrouns-sizeのみの指定をするか）
		画像に対してのrepeat position, (複数の場合の例: '0 0 no-repeat','left top repeat')
		'!important', //!importantを指定したい場合（必要ない場合は空）
		true or false, // base64にするかしないか
		true or false, // Retina対応（画像サイズを半分に）するかしないか
	);

##### 初期設定
	@include background-fit(
		'',
		true,
		'0 0 no-repeat',
		'',
		$isBase64, // 予め_setting.scssで設定されてることを考慮
		$isRetina // 上に同じ
	);



##### 例
###### SCSS
	@include background-fit(
		("text.png",linear-gradient(rgba(red,0.1),rgba(red,0.3))),
		false,
		('center 5px no-repeat','0 0 no-repeat')
	);

###### CSS 
	background: url('/html/img/text.png?1396155926') center 5px no-repeat, linear-gradient(rgba(255, 0, 0, 0.1), rgba(255, 0, 0, 0.3)) 0 0 no-repeat;
	background-size: 327px 29px, auto auto;
	

### half-image-width
画像の半分の横幅の値を返す（複数画像を配列で指定可能）

#### 使い方

1. 関数実行後$imgWidthの中に画像のサイズが入ります。<br>複数の場合配列に入るのでnth($imgWidth,1)のように取ってくることができます。

##### 例
###### SCSS
	width: half-image-width('test.png');

###### CSS 
	width: 100px;

### half-image-height
画像の半分の高さの値を返す（複数画像を配列で指定可能）

#### 使い方

1. 関数実行後$imgHeightの中に画像のサイズが入ります。<br>複数の場合配列に入るのでnth($imgHeight,1)のように取ってくることができます。

##### 例
###### SCSS
	height: half-image-width('test.png');

###### CSS 
	height: 100px;

### sprite-info
スプライト画像のサイズとポジションを設定する
#### 例
##### SCSS
	$map: sprite-map('num/*.png', $spacing: 5px);
	p {
		@include sprite-info($map,img3);
	}
##### CSS
	background-image: url('/html/img/num-sce8bc88143.png');
	background-size: 50px 270px;
	background-position: 0 0;
	width: 50px;
	height: 50px;

### text-shadow-repeat
同じテキストシャドウを繰り返したい場合
#### 例
##### SCSS
	@include text-shadow-repeat(0 0 3px #000,5);
##### CSS
	text-shadow: 0 0 3px black,0 0 3px black,0 0 3px black,0 0 3px black,0 0 3px black;

### box-shadow-repeat
同じテキストシャドウを繰り返したい場合
#### 例
##### SCSS
	@include box-shadow-repeat(0 0 10px #fff,10);
##### CSS
	box-shadow: 0 0 10px black,0 0 10px black,0 0 10px black,0 0 10px black,0 0 10px black;

### display-box-center
display: box;にし縦横をcenter寄せにする。
webkitに対応したサイトを作る際に役立ちます。
#### 例
##### SCSS
	@include display-box-center();
##### CSS
	display: box;
	box-align: center;
	box-pack: center;


### triangle
三角形を作る

#### 使い方

##### 初期設定・説明
	@include triangle-set(
		10px, //大きさ: 10px 20px のように2つ値を書くことで横と高さを指定することができます。
		black, // 色
		top //向き: top,right,bottom,left
	);


### triangle-set
作った三角形を向きに合わせてボックスの外側にセットする（吹き出し風になるようにセットする）

#### 使い方
1. 吹き出し風にしたい場合にtriangleを使わずこっちを使う。

##### 初期設定・説明
	@include triangle-set(
		10px, //大きさ: 10px 20px のように2つ値を書くことで横と高さを指定することができます。
		black, // 色
		top, // 向き: top,right,bottom,left
		50%, // 位置: 三角形をボックスのどの位置にセットするか（真ん中の場合50%）
		after // before or after どちらを使うか
	);

### arrow
矢印を作る
#### 使い方

##### 初期設定・説明
	@include arrow(
		10px, // 大きさ
		1px, // 太さ
		white, // 色
		top, // 向き: top,right,bottom,left
	);

### arrow-set
作った矢印を向きに合わせてボックスにセットする

#### 使い方

##### 初期設定・説明
	@include arrow-set(
		10px, // 大きさ
		1px, // 太さ
		white, // 色
		top, // 向き: top,right,bottom,left
		50%, // 位置: 三角形をボックスのどの位置にセットするか（真ん中の場合50%）
		after // before or after どちらを使うか
	);

### background-4corners
4つ角に同じ画像を回転・反転させておきたい場合

##### 初期設定・説明
	@include background-4corners(
		'', //images_dirで指定したパスからの画像ファイル名
		$isRetina // true or false Retina対応（画像サイズを半分に）するかしないか
					（初期値は予め_setting.scssで設定されてることを考慮）
	);

### es
基準となるサイズに対する値を、さまざまな単位にエンコード（encode size）

#### 使い方

##### 初期設定・説明
	margin: es(
		'', // 変換したいサイズ
		$font-unit, // 変換したい単位（%,em,rem,px）（初期値は予め_setting.scssで設定されてることを考慮）
		$fontSizeDefault // 変換の基準となる値（初期値は予め_setting.scssで設定されてることを考慮）
	);

##### 例
例えば16pxのサイズのspanを12pxの%に変換したい場合
###### SCSS

	font-size: es(12,'%',16);
	//または
	$font-unit: '%';
	$fontSizeDefault: 16;
	font-size: es(12);
###### CSS
	font-size: 75%;


### fz
指定した単位にフォントサイズを変換しfont-sizeを代入する


#### 仕様・使い方
* remの場合remが使えない場合のためにpxも出力する

##### 初期設定・説明
	＠include fz(
		'', // 変換したいサイズ
		'', //!importantを指定したい場合（必要ない場合は空）
		$font-unit, // 変換したい単位（%,em,rem,px）（初期値は予め_setting.scssで設定されてることを考慮）
		$fontSizeDefault // 変換の基準となる値（初期値は予め_setting.scssで設定されてることを考慮）
	);


##### 例
例えば16pxのサイズのspanを12pxの%に変換したい場合
###### SCSS
	@include fz(12,'',rem,10);
	//または
	$font-unit: rem;
	$fontSizeDefault: 10;
	@include fz(12);
###### CSS
	font-size: 12px;
	font-size: 1.2rem;

### golden-ratio
黄金比の計算
##### 初期設定・説明
	＠include golden-ratio(
		0, // この数字に対しての黄金比（$num * 1.618）
		false, // trueの場合 第1引き数の数字を元にした（$num / 1.618）
	);

### silver-ratio
白銀比の計算
##### 初期設定・説明
	＠include silver-ratio(
		0, // この数字に対しての黄金比（$num * 1.414）
		false, // trueの場合 第1引き数の数字を元にした（$num / 1.414）
	);

### clearfix
引き数内のクラスにclearfixを付ける

#### 使い方
1. <code>@include clearfix();</code>とかくとclearfixクラスがclearfixとなるcssが出力される。
2. <code>@extend %clearfix;</code>することによりクラスを追加していく。

###### SCSS
	@include clearfix();

	.test {
		@extend %clearfix;
	}
###### CSS
	.clearfix,
	.test {
		min-height: 1px;
		_height: 1%;
	}
	.clearfix:after,
	.test:after {
		content: ".";
		display: block;
		clear: both;
		height: 0;
		visibility: hidden;
	}


ライセンス
----------
+ Copyright 2014 &copy; kamem
+ [http://www.opensource.org/licenses/mit-license.php][mit]

[MIT]: http://www.opensource.org/licenses/mit-license.php
