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
