## Swift文法4

### オプショナル型
---

#### null
Swiftでは**null**ではなく**nil**を使います。
ただしswiftのnilは扱うべき値が存在しないことを表します。
C言語のポインタの指す場所が無いnullポインタとは厳密には意味が違います。

#### オプショナルな変数(Option Value)
Swiftでは変数宣言時に値を代入していなくてもコンパイルエラーになりません。

しかし、
~~~
var required: String
println(required)     // コンパイルエラー
~~~
変数に値を入れる前に参照しようとするとコンパイルエラーになります。

このように通常の変数には中身が入っている（nilでない）ことをコンパーラーが保証してくれています。
よって、従来の言語のように参照する前にnullチェックなどをする必要がなくなりました。

通常の変数では上記の理由で**nil**を代入することはできません。
nilを代入したい場合は **オプショナルな変数(Optional value)** にする必要があります。
オプショナルな変数には型の後に **?** を記述します。
~~~
var required: String = "必須"
var optional: String? = "オプチョナル"

required = nil    // コンパイルエラー
optional = nil    // こっちは大丈夫
~~~

通常の変数にオプショナルな変数を代入しようとするとエラーになります。
これを回避するためにはオプショナル変数の後ろに **!** をつける必要があります。
このことを **開示(unwrap)** といいます。
~~~
var required: String = "必須"
var optional: String? = nil

required = optional     // コンパイルエラー
required = optional!    // 後ろに!があるとコンパイルエラーにならない
                        // ただし、実行時にエラーとなる
~~~
!をつけてしまうとコンパイラーの恩恵を受けられないので、あまり使ってはいけないケースだと思われる。

他の回避方法として、参照予定のオプショナルな変数がnilだった場合はデフォルトの値を使うように指定する **??** 演算子を使用することができます。

~~~
let nick_name: String? = nil;
let full_name = "John Appleseed";

let informal_greeting = "Hi \(nick_name ?? full_name).";   // Hi John Appleseed
// nick_nameがnilのためfull_nameが参照される
~~~

#### オプション型の条件式
オプショナル型の変数がどうかを調べるには比較演算子を使用します。
~~~
let value: Int? = nil

print(value == nil)		// true
print(value != nil)		// false

// 大小比較も可能。ただし、nilはどんな数や文字列より小さいという結果が返される。
print(value <= 100)		// true
print(value >= 100)		// false

// if文でも使用可能
if value == nil {		// 条件式の結果は真(true)
	print("valueはnil")
}
else {
	print("valueはnilでない")
}
~~~

### オプション束縛構文(optional binding)
---

#### if-let文
オプショナル型の変数がnilでなかった場合、その値をすぐに使いたいことがあります。
この時便利の記法が用意されています。
~~~
// if-let文
if let y = olympic {			// 条件式の結果は真(true)
	print("あと\(y - 2016)年")	// "あと４年"
}
else {
	print("エラー")
}

// 省略版
if let y = Int("2020") {
	print("あと\(y - 2016)年")
}
else {
	print("エラー")
}
~~~
上記の例でもし変数olympicにnilが入っていた場合、「エラー」が出力されます。

**この構文はif文とwhile文の時のみ使用できます。**

#### guard文
オプショナル束縛に限定した構文では無いですが、
想定外の状況（参照する変数がnilなど）が発生した場合にその処理を抜け出すための構文として **guard文** があります。
guard文の概要を下記に示します。
~~~
guard 条件式 else {
    /* breakやreturn */
}
~~~
条件式の条件が **成立しなかった場合** にelse節が実行され、そのコードブロックから抜け出します。
else節には通常の処理を記述できますが、
必ず **guard文も含む現在実行中のコードブロックから抜け出す文を記述しないといけません。(breakやreturnなど)**　
使用例を下記に示します。
~~~
let stock = [ "01", "2", "4", "05", "8", "q", "X" ]
for str in stock {
	// guard文
	guard let v = Int(str) else {	// 条件が成立しなかった場合に実行する
		print(str + "??")
		// for文を抜ける
		break						// 必須！ ないとコンパイルエラーになる
	}
	print(v, terminator:" ")		// guard文の条件節で宣言した定数が利用できる
}
// "1 2 4 5 8 q??"
~~~

if文を使用すれば同様の処理は可能ですが、~~guard文を使用することでエラーチェックの条件だと認識しやすく可読性が増すと思われます。~~

#### nil合体演算子
実際の処理で、オプショナル変数opvがnilでなければopvを開示して使用するが、nilだった場合はSを使うといったことがあります。
上記の処理を三項演算子で記述すると下記の処理になります。
~~~
(opv != nil) ? opv! : S
~~~

Swiftでは、上記の簡単に記述するための **nil合体演算子(nil coalescing operator)** が用意されています。
nil合体演算子は **??** を記述します。
~~~
opv ?? S
~~~

使用例としては、文字列strを数値に変換したい場合、変換に失敗した場合は値0を入れる処理を下記に示します。
~~~
var number = Int(str) ?? 0
~~~

演算子??は複数記述することができます。
~~~
var number = x ?? y ?? x ?? 0
~~~
上記の例は、xの値がnilでなければxの値を、そうでなければ同様にy、zの順番で調べ、
いずれもnilだったら0を返します。

### 失敗のあるイニシャライザ
---

#### 概要
イニシャライザは構造体やクラスのインスタンス初期化に用いられる機能です。
イニシャライザの使い方として、与えられた引数の値を使用しインスタンスを初期化しますが、
この手続きが成功するとは限りません。
例えば、時間を表す構造体のイニシャライザに1時63分といった値でイニシャライザをした場合、
初期化が成功したとは言えません。
そこで、初期化が失敗した場合にnilを返す **失敗のあるイニシャライザ(failable initializer)** を利用します。

#### 失敗のあるイニシャライザの定義
失敗のあるイニシャライザの構文を下記に示します。
~~~
init? (/*引数(省略可能)*/) {
    return nil      // 失敗時にこれで初期化を終了させる。
}
~~~
**return nil** は必ず失敗のあるイニシャライザに含めなければなりません。
またnil以外の値を返せません。
もし、失敗のあるイニシャライザで **return nil** がなかった場合、コンパイルエラーとなります。

時間を表す構造体に対して失敗のあるイニシャライザを使用した例を下記に示す。
~~~
struct Time {
	let is24h: Bool
	var hour = 0
	var min = 0

	init? (_ h: Int, _ m: Int, is24h: Bool = false) {		// 失敗のあるイニシャライザ
		let maxh = is24h ? 23 : 11
		if (h < 0) || (h > maxh) || (m < 0) || (m > 59) {
			// 初期化に失敗した場合
			return nil				// 初期化失敗のケースで、必ずnilを返さなければならない
		}
		self.is24h = is24h
		hour = h
		min = m
	}

	init(time: Time, is24h: Bool) {
		var h = time.hour
		if !is24h && time.hour > 11 {
			h -= 11
		}
		self.is24h = is24h
		hour = time.hour
		min = time.min
	}
}

if let w = Time(22, 10, is24h: true) {	// オプショナル束縛構文
	print ("\(w.hour):\(w.min)")	// "22:10"
}
var u: Time = Time(23, 40)!		// 既定値は12時制なので、エラー発生
var t:Time? = Time(20, 0, is24h: true)	// オプショナル型の変数tに格納
u = Time(time: t!, is24h: false)	// 変数tはオプショナル型なので!が必要
~~~
