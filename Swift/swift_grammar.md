## swift文法

### 変数宣言
---
~~~
var number = 0;        // 変数宣言
var number: Int = 0;   // 型を明記した場合
~~~

### 定数宣言
---
~~~
let number = 0    // 定数宣言
let number: Int = 0;   // 型を明記した場合
~~~

### 型の種類
---
| 型 | 型の説明 |
| :-----: | -------|
| Int | 整数型 |
| Double | 浮動小数点数型(64bit長) |
| Float | 浮動小数点数型(32bit長) |
| Bool | 論理型 (true or false) |
| String | 文字列型 |
＊ 型の頭文字は大文字

### 算術
---
注意点：
swift3からインクリメントとデクリメントの文法は除去されます。
~~~
var a = 1
a++         // swift2ではwarnningが表示される
a           // swift2ではwarnningが表示される
~~~
よって、swiftでインクリメントしたい場合は下記の文法に置き換える必要がある。
~~~
var a = 1
a += 1
a -= 1
~~~

### 範囲演算子
---
範囲演算子とはその名の通り値の範囲を示すoperatorです。
範囲演算子は二つあります。
* Closed range operator
      * a...b   (a <= X <= b)
* Half-open range operator
      * a..<b   (a <= X < b)

具体的な使用例を下記のコードに示す。
~~~
// Closed range operatorの使用例
for index in 0...5 {
  print("indexは\(index)です")
}
// 結果：
//indexは0です
//indexは1です
//indexは2です
//indexは3です
//indexは4です
//indexは5です

// Half-open range operatorの使用例
for index in 0..<5 {
    print("indexは\(index)です")
}
// 結果：
//indexは0です
//indexは1です
//indexは2です
//indexは3です
//indexは4です
~~~

範囲を表す数字を変数やマイナス値に置き換えることもできる。
~~~
// 変数を使用した場合
var start = 0
var end = 5
for index in start...end
{
  print("indexは\(index)です")
}

// マイナス値
for index in -6...(-1)    // 後ろの数字がマイナスの場合は()が必要
{
    print("\(index)")
}
~~~


### 型変換
---
swiftは**暗黙的な型変換は行いません**。
異なる型に変換したい場合は変換先の型を明記する必要があります。

~~~
let label = "The width is "
let width = 94
let width_label = label + String(width)   // "The width is 94"
~~~

Stringに変数の値を含めたい場合は下記の方法もある。

~~~
let apples = 3
let apple_summary = "I have \(apples) apples."
// "I have 3 apples."　
~~~

### コレクション型
---

#### 配列(Array)
swiftでの配列(Array)は、 ~~同じ型を持つ値を順番に格納することができます。~~

違う型をいれてもコンパイルエラーにならない・・・。

まず、初期値あり配列の宣言方法を下記に示します。
~~~
var array_int1: Array<int> = [10, 7, 11]   // (1)
var array_int2: [Int] = [10, 7, 11]        // (2)
var array_int3: Array = [10, 7, 11]        // (3)
var array_int4 = [10, 7, 11]               // (4)
~~~
4つの宣言方法を記述しましたが結果は全て同じです。(1)Array< type >はSwiftの文法的に
最も正しい書き方だと思われます。(2)の書き方は(1)を省略したものです
（Apple公式のswiftマニュアルではこの記述方法を使用している）
(3)は型の指定を省略した形です。右辺の値の型推論から型が確定します。
(4)はすべてを省略した最も短い宣言方法です。

### 条件分岐
---

### 繰り返し文
---

#### for
例を下記に示す
~~~
var total = 0
for i in 0..<4 {    // 4回(0〜3)繰り返す
    total += i
}
~~~
C言語のような条件式を記述することもできますがswift3で使えなくなるため、 **for in**を使うことを推奨する。

#### while
例を下記に示す。
~~~
var n = 1
while n < 100 {
    n = n * 2
}
~~~

#### repeat-while (do-while)
後判定ループ（C言語ではdo-while）をswiftで使う場合はrepeat-whileを使用する。
例を下記に示す。
~~~
var m = 2
repeat {
    m = m * 2
} while m < 100
~~~

### switch文
---
switch文の分岐条件は整数型以外でも使用可能。
~~~
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):   // 語尾が"pepper"であるのでここに分岐する。
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
~~~

swiftのswitch文ではdefaultを記述しなくてはならない。記述しなかった場合はコンパイルエラーとなる。

また条件に分岐した後、次項のcaseには分岐しないため、case毎にbreak文を書く必要がありません。

### null
---

swiftでは**null**ではなく**nil**を使う

### オプショナルな変数(Option Value)
---

swiftでは変数宣言時に値を代入していなくてもコンパイルエラーになりません。

しかし、
~~~
var required: String
println(required)     // コンパイルエラー
~~~
変数に値を入れる前に参照しようとするとコンパイルエラーになる。

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
~~~
var required: String = "必須"
var optional: String? = nil

required = optional     // コンパイルエラー
required = optional!    // 後ろに!があるとコンパイルエラーにならない
                        // ただし、実行時にエラーとなる
~~~
!をつけてしまうとコンパイラーの恩恵を受けられないので、あまり使ってはいけないケースだと思われる。

他の回避方法として、参照予定のオプショナルな変数がnilだった場合はデフォルトの値を使うように指定する **??** 演算子を使用することができる。

~~~
let nick_name: String? = nil;
let full_name = "John Appleseed";

let informal_greeting = "Hi \(nick_name ?? full_name).";   // Hi John Appleseed
// nick_nameがnilのためfull_nameが参照される

~~~


### 関数
swiftで関数を作成すると下記のようになる。
~~~
func greet(name: String, day: String) -> String {
    return "Hello \(name), today is \(day)."
}

print(greet("Bob", day: "Tuesday"))     // Hello Bob, today is Tuesday
~~~
関数の戻り値の型は **->** で指定する。
