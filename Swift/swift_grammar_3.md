## swift文法3

### 構造体
---
swiftの構造体はC言語と比べて様々な機能が追加されており、
メソッドやプロパティ（変数や定数）を定義することができます。

#### 構造体の宣言
構造体の宣言はC言語と同様structキーワードを使用します。
例では簡単な構造体の宣言と使用例をしまします。
~~~
// 構造体の使用例
struct Time {
	var hour: Int	// 時間
	var	minute: Int	// 分
	var second: Int	// 秒
}

var now = Time(hour: 8, minute: 25, second: 55)

// ちなみにprintも使用することができる
print(now)	// "Time(hour: 8, minute: 25, second: 55)"
~~~

#### 定義の概要
概要でも説明しましたが、swiftの構造体にはメソッドやプロパティを定義することができ、
クラスと使い方が似ています。
構造体とクラスの違いは、構造体は値型のデータであることと継承ができないということです。
対してクラスは参照型であり継承が可能です。
継承はオブジェクト指向の機能であるので割愛します。
値型と参照型については下記のソースコードを参照してください。
~~~
// 構造体
struct UserStruct {
	var age: Int

	init (age: Int){
		self.age = age
	}
}

// クラス
class UserClass {
	var age: Int

	init (age: Int){
		self.age = age
	}
}

var userStruct1 = UserStruct(age: 30)
var userStruct2 = userStruct1
// 新しい構造体として代入されるので、userStruct1には影響しない
userStruct2.age = 32
print(userStruct1.age) // 30
print(userStruct2.age) // 32

var userClass1 = UserClass(age: 30)
var userClass2 = userClass1
// クラスは参照型なので、userClass1に影響する
userClass2.age = 32
print(userClass1.age) // 32
print(userClass2.age) // 32
~~~
構造体とクラスの使い分けについて、構造体を使用する場合は
* データが小さい場合（大きいと速度が遅くなる）
* 継承が必要ない場合
* 明確に値渡しがいい場合

アップルが公開しているThe Swift Programming Language (Swift 2.2)でも
クラスではなく構造体を使用する際のガイドラインを下記のように記述しています。
> * The structure’s primary purpose is to encapsulate a few relatively simple data values.
>
> * It is reasonable to expect that the encapsulated values will be copied rather than referenced when you assign or pass around an instance of that structure.
>
> * Any properties stored by the structure are themselves value types, which would also be expected to be copied rather than referenced.
>
> * The structure does not need to inherit properties or behavior from another existing type.

※ [The Swift Programming Language (Swift 2.2) | Classes and Structures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-ID82) : Choosing Between Classes and Structures参照

要約すると
* 数個の単純な値をまとめたい場合
* 構造体のインスタンスを受け渡すとき、参照渡しではなく値渡しの方が合理的だと思われる場合
* 構造体に格納されているプロパティが値型であり、そのプロパティが参照渡しではなく値渡しが望ましい場合
* 他からプロパティや動作（メソッド）を継承する必要がない場合

となっています。

上記の条件に当てはまらない場合は、クラスを使う方が望ましいと思われます。


#### 構造体の初期値
構造体のインスタンス生成の際、初期化のために使用する手続きを **イニシャライザ** と呼びます。
イニシャライザには複数種類があり、用途によって使い分けます。

##### ・規定イニシャライザ
まず構造体の定義の中で各プロパティに値を代入しておく **規定イニシャライザ(default initializer)** があります。
すべてのプロパティに規定イニシャライザが定められている場合、インスタンス生成時に実引数なしで宣言することが可能です。
~~~
// 構造体：規定イニシャライザ
struct Date
{
	var year = 2016	// 2016で初期化
	var month = 6	// 6で初期化
	var day = 27	// 27で初期化
}

// 実引数なしでインスタンス生成
var today = Date()		// 2016年6月27日
var tomorrow = Date()	// todayと同じ日付
~~~

##### ・全項目イニシャライザ
もしインスタンスを生成したときに各プロパティに任意の値を割り当てたい場合、
インスタンス生成時に実引数を設定する方法があります。
これを **全項目イニシャライザ(memberwise initializer)** と呼びます。
~~~
// 構造体：全項目イニシャライザ（規定イニシャライザあり)
struct Date
{
	var year = 2016	// 2016で初期化
	var month = 6	// 6で初期化
	var day = 27	// 27で初期化
}
// 規定イニシャライザの値は上書きされる
var today = Date(year: 1991, month: 1, day: 1)		// 1991年1月1日
var yesterday = Date(year: 2000, month: 9, day: 11)	// 2000年9月11日

// 構造体：全項目イニシャライザ（規定イニシャライザなし)
struct Time {
	// 規定値がない場合は型を指定しなくてはいけない
	var hour: Int
	var	minute: Int
	var second: Int
}
var now = Time(hour: 11, minute: 50, second: 23)	// 11時50分23秒
//var past = Time()		// コンパイルエラー : 規定値がない場合は全項目イニシャライザを使用しなくてはならない
~~~

もしプロパティに定数(let)があった場合、その定数が規定イニシャライズされているかどうかで全項目イニシャライザの記述内容が変わってきます。
~~~
struct Time1 {
	let is24h: Bool = false
	var hour: Int
	var	minute: Int
}
//var past = Time1(is24h: true, hour: 3, minute: 15)	// コンパイルエラー
// 定数(is24h)にはすでに値が入っているので書き換えられない

struct Time2 {
	let is24H: Bool
	var hour: Int
	var minute: Int
}
var now = Time2(is24H: true, hour: 23, minute: 22)	// 定数(is24h)には値が入っていないためこっちは問題ない
~~~

##### ・initによるイニシャライザ
初期化の処理をC++やJavaのコンストラクタのように関数で行うことができます。
その場合、funcや関数名を書かずに **init** というキーワードを記述しイニシャライザであることを宣言します。
構造体のメソッドを使用する場合、このinitの処理が終わらないと使用することができません（init内で構造体のメソッドを使用することはできません）。
init内で自身のプロパティを使う場合、 **self** (Javaのthisみたいなもの)を使う必要があります。
~~~
struct Time {
	var hour: Int
	var    minute: Int
	var second: Int

	// ①引数なし
	init() {
		// selfは省略可能
		hour = 24
		minute = 0
		second = 0
	}

	// ②引数あり（一部）
	init(hour: Int) {
		self.hour = hour
		// 引数と変数名が同じでなかった場合、selfは省略できる
		minute = 0
		second = 0
	}

	// ③引数あり
	init(hour: Int, min: Int, sec: Int) {
		self.hour = hour
		self.minute = min
		self.second = sec
	}
}

var time1 = Time()								// ①引数無し
var time2 = Time(hour: 24)						// ②引数あり（一部）
var time3 = Time(hour: 12, min: 22, sec: 31)	// ③引数あり
~~~
例のように引数有る無しで複数のinitを作成することができます。
