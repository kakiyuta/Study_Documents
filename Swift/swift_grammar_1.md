## Swift文法1

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

### print関数
---
printとはSwiftでの標準出力関数です。
C言語ではprintfに相当します。
printは指定しなければ自動的に改行されます。文字列終端に'\n'を入力する必要はありません。
~~~
print("Hello world")    // "Hello world"

var number = 100
print(number)   // "100"

var str = "hogehoge"
print(str)      // "hoeghoge"
~~~

引数が複数ある場合は","で区切ります。
","で区切るとデフォルトでは空白が入力されるが、 **separator:** を使用することで
区切り文字を指定することができます。
またデフォルトで出力する文字列の最後に改行が入るが、 **terminator:** で改行以外の終端文字を
指定することができます。
~~~
print(2016, 6, 19)    // "2016 6 19"

print(2016, 6, 19, separator:"/")   // "2016/6/19"

print(2016, 6, 19, separator:"/", terminator:"です")  // "2016/6/19です"（改行なし）
~~~

C言語のprintfと同様に  "\n"(改行) や "\t"(タブ) を使用することもできます。

### readLine関数
Swiftの出力用関数としてはprintが用意されていますが、入力用関数は **readLine関数** 使います。
readLine()は標準入力からのデータをUTF-8として解釈し、一行文の文字列を読み込んで返します。
返り値の方はString?でEOFの場合はnilを返します。(Stringの **?** と **nil** についてはオプショナル型で説明します。)

readLine()には既定値のある引数stripNewLine:があります。
この引数は行末の改行文字を入力から取り除くかどうかをBool型の値で指定します。
既定値ではtrueで、つまり改行文字が取り除かれます。
~~~
let strNotEnter: String? = readLine();		// 改行文字は取り除かれる。
print(strNotEnter!)

let strEnter: String? = readLine(stripNewline: false)	// 改行文字は読み込まれる。
print(strEnter!)
print("----------------------------")
~~~
print関数の引数についてある **!** はオプショナル型で説明します。

### =演算子（代入演算子）
Swiftの=演算子（代入演算子）は **値を返さない(値の更新のみ)** と定義されています。
よって、C言語のように
~~~
a = b = c
~~~
と記述することはできません。

これにより、if文などで **=** と **==** を間違えることがなくなります。
~~~
if i = 0 {    // コンパイルエラー
}
~~~

### 算術
---
注意点：
Swift3からインクリメントとデクリメントの文法は除去されます。
~~~
var a = 1
a++         // Swift2ではwarnningが表示される
a--         // Swift2ではwarnningが表示される
~~~
よって、Swiftでインクリメントしたい場合は下記の文法に置き換える必要がります。
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
      a...b   (a <= X <= b)
* Half-open range operator
      a..<b   (a <= X < b)

例のaとbの間にスペースを入れることはできません。
具体的な使用例を下記のコードに示します。
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

範囲を表す数字を変数やマイナス値に置き換えることもできます。
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
Swiftは**暗黙的な型変換は行いません**。
例えばIntとDoubleの変数で演算する場合、型が違うのでコンパイルエラーになります。
異なる型に変換したい場合は変換先の型を明記する必要があります。

~~~
let label = "The width is "
let width = 94
let width_label = label + String(width)   // "The width is 94"
~~~

Stringに変数の値を含めたい場合は下記の方法もあります。

~~~
let apples = 3
let apple_summary = "I have \(apples) apples."
// "I have 3 apples."　
~~~

### 数値リテラル
---
Swiftでは例のように数値リテラルを表現することができます。
~~~
let decimal_integer = 17
let binary_integer = 0b10001    // 17の2進数
let octal_integer = 0o21        // 17の8進数
let hexadecimal_integer = 0x11  // 17の16進数
~~~

### 数値の範囲判定
---
Swiftでは値が型の範囲に収まっているかを判定してくれます。
~~~
let cannot_be_negative: UInt8 = -1    // コンパイルエラー
// UInt8は符号なし整数なので、-1を入れることができない

let too_big: Int8 = Int8.max + 1      // コンパイルエラー
// Int8の範囲外の数値を代入することはできない

var int_max: Int8 = Int8.max    // 8bit符号付き整数の最大値
var one = 1
var sum = int_max + one         // コンパイルエラーにならない
// この場合はコンパイルエラーにならないが、実行時にエラーとなりプログラムが停止する。
~~~

### コレクション型
---

#### 配列(Array)
Swiftでの配列(Array)は、 ~~同じ型を持つ値を順番に格納することができます。~~

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
（Apple公式のSwiftマニュアルではこの記述方法を使用している）
(3)は型の指定を省略した形です。右辺の値の型推論から型が確定します。
(4)はすべてを省略した最も短い宣言方法です。

配列の使用例を下記の例を用いて説明します。
~~~
var str_empty1 = [String]()     // String型の空配列宣言(方法1)
var str_empty2: [String]        // String型の空配列宣言(方法2)

// 配列要素[]の中に数式を記述することができる。
var g = 1.2
var f = [g/2.0, g/3.0, g/4.0]

// print関数に配列の変数を渡すと丸ごと出力される
print(f)        // [0.6, 0.4, 0.3]
// 自分の環境で実行すると[0.59999999999999998, 0.39999999999999997, 0.29999999999999999]となってしまう。

// 配列の各要素にアクセスするには[]と添字(0から始まる整数)を使う("[]は表示されない")
print(f[1])     //　0.4
//print(f[3])   // 配列の範囲外を指定すると実行時にエラーとなりプログラムが停止する

// 配列名.countで配列の要素数を取得できる
print(f.count)      // 3

var nums = f        // 配列がコピーされる
nums[2] = 3.14      // 変数には代入可能
//f[2] = 3.14       // コンパイルエラー：定数配列には代入できない
print(f[2])         // 0.3 (配列コピーのためコピー元に影響はない)
~~~

配列の末尾に新しい要素を追加する場合はappendというメソッドを使います。
また演算子+や+=を使用することもできます。
~~~
var roman = ["I", "II", "III"]  // String型配列
roman.append("IV")              // 末尾に"IV"を追加
print(roman)            // ["I", "II", "III", "IV"]

let m = roman + ["5", "6"]
print(m)        // ["I", "II", "III", "IV", "5", "6"]

roman += ["V", "VI"]
print(roman)    // ["I", "II", "III", "IV", "V", "VI"]
~~~

### タプル
---
タプルは複数の値を格納することができます。
~~~
let http_404_error = (404, "Not Found")
~~~
例ではhttpのエラーコード(Int型)とエラーメッセージ(String型)を格納しているタプル(http_404_error)
を宣言しています。

宣言したタプルの各値を別々の変数または定数に入れることができる。
~~~
let (error_code, error_message) = http_404_error

print("Error code is \(error_code)")
// Error code is 404
print("Error message is \(error_message)")
//  Error message is Not Found
~~~

上記の例で、もしエラーコード(404)だけを取得したい場合は、不要な部分をアンダースコアにすることができます。
~~~
let (error_code, _) = http_404_error
// 404だけをerror_codeに代入
~~~

タプルのインデックス番号を指定して、各要素を取得してくることもできます。
~~~
print("Error code is \(http_404_error.0)")
// Error code is 404
print("Error message is \(http_404_error.1)")
//  Error message is Not Found
~~~

タプルの宣言時に、各要素に名前をつけることができる。
各要素を取得したい場合はその名前を指定するだけで取得する。
~~~
let http_404_error = (code: 404, message: "Not Found")

print("Error code is \(http_404_error.code)")
// Error code is 404
print("Error message is \(http_404_error.message)")
//  Error message is Not Found
~~~

タプルを利用することで、関数の戻り値に複数の値をreturnすることができます。

### 条件分岐
---
if文の分岐条件ではBool(true or false)でなければなりません。
~~~
lei i = 1
if i
{
  // iはInt型のためコンパイルエラーになる
}

let i = 1
if i == 1
{
  // 比較演算子==の演算結果がBool型になるので問題なし
}
~~~

また、括弧{}は省略できません。

### 繰り返し文
---

#### for
例を下記に示します
~~~
var total = 0
for i in 0..<4 {    // 4回(0〜3)繰り返す
    total += i
}
~~~
iは自動的に定数となり、繰り返しの内部のみ有効となります。
よってforの直後にletやvarは書くことはできません。

C言語のような条件式を記述することもできますがSwift3で使えなくなるため、 **for in**を使うことを推奨します。

for-in文にはオプションでwhere節を記述し、その条件に当てはまった時だけコードブロックが実行されます。
~~~
for i in 1..<64 where i % 3 != 0 && i % 8 != 0
{
    // i が3の倍数でない かつ 8の倍数でない時に出力される
    print(i)
}
~~~

#### while
例を下記に示します。
~~~
var n = 1
while n < 100 {
    n = n * 2
}
~~~

#### repeat-while (do-while)
後判定ループ（C言語ではdo-while）をSwiftで使う場合はrepeat-whileを使用します。
~~~
var m = 2
repeat {
    m = m * 2
} while m < 100
~~~

### ラベル付きループ文
---
ラベル付きループ文とは、多重ループ抜ける手法の一つでgoto文に似ています。（Swiftではgoto文はありません）

多重ループを構成するwhile文やfor文などにラベルを付けておき、break文とcontinue文を使用する際に
どのループを抜けるか、またはどのループの実行を継続するかをラベルで指定します。
~~~
let days = 31           // 1ヶ月の日数
let firstDay = 2        // 1日目の曜日(0:日曜日)
for i in 0..<firstDay   // 月初めに空白を入れる
{
    print("    ", terminator:"") // 改行なし
}

var d = 1               // 日にちを表す変数
var week = firstDay     // 曜日のための変数

loop: while true
{
    while week < 7
    {
        // 一桁数字の場合の表示調整
        let pad = d < 10 ? " " : ""
        print(pad + "  \(d)", terminator:"")

        // 月末判定
        d += 1
        if d > days
        {
            // 改行
            print("")
            break loop
        }

        week += 1
    }
    // 改行
    print("")
    week = 0
}

~~~


### switch文
---
switch文の分岐条件は整数型以外でも使用可能です。
~~~
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":            // 複数を列挙することもできる
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):   // 語尾が"pepper"であるのでここに分岐する。
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
~~~

Swiftのswitch文ではdefaultを記述しなくてはなりませ。記述しなかった場合はコンパイルエラーとなります。

また条件に分岐した後、次項のcaseには分岐しないため、case毎にbreak文を書く必要がありません。
ただしcase処理の途中でswitch文から抜けたい場合は **break** を使用することができます。

もしC言語のように下のcase文の処理を実行したい場合は **fallthrough** という文を
case文の一番下に記述すれば、一個下のcase文も実行されます。
~~~
let number = 0;
switch number
{
case 0:
    print("0")
    fallthrough       // 下のcase処理も実行する
case 1:
    print("1")
default:
    print("default")
}
// "0"と"1"が出力される。
~~~
