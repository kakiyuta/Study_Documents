## Swift文法2

### 関数
---

#### 基本文法
Swiftで関数を作成すると下記のようになります。
~~~
func greet(name: String, day: String) -> String {
    return "Hello \(name), today is \(day)."
}

print(greet("Bob", day: "Tuesday"))     // Hello Bob, today is Tuesday
~~~
関数の戻り値の型は **->** で指定します。

#### 外部引数名
Swiftでは、関数の呼び出し側で引数の名前を指定する必要があります。
例では第一引数は名前指定を省略でいますが、第二引数からは名前指定（デフォルトでは引数の変数名）を
指定しなくてはいけません。この名前指定を **外部引数名** と言います。

外部引数名は実装者が任意の名前を付けることができます。引数に名前を付けることにより、
関数の呼び出し側でも引数として渡すべき値の意味を理解できるという利点があります。
~~~
// 引数の高さと幅で面積を算出する関数
func area(height h: Int, width w: Int) -> Int
{
    return h * w
}

let takasa = 10     // 高さ
let haba = 5        // 幅
let menseki = area(height: takasa, width: haba) // 面積
print("面積は\(menseki)です")
~~~
関数area()には引数が二つあり、それぞれにheight, widthと外部引数名を付けています。
呼び出し側が関数を使う場合は、引数の値とその外部引数名も記述する必要があります。

外部引数名の指定が手間な場合は'_'(アンダースコア)を使用することで、
記述を省略を省略できます。area関数の外部引数を省略する場合は
~~~
// 引数の高さと幅で面積を算出する関数
func area(h: Int, _ w: Int) -> Int
{
    return h * w
}

let takasa = 10     // 高さ
let haba = 5        // 幅
let menseki = area(takasa, haba) // 面積
print("面積は\(menseki)です")
~~~
となります。
第一引数はもともと省略可能なので、第二引数のみ'_'を使用すれば呼び出し側で外部引数名を
記述する必要がなくなります。

#### inout引数
C言語で引数で渡した値が関数内で変更したい場合、ポインタを利用して渡します。
しかし、Swiftにはポインタがありません。
Swiftで関数内で引数の値を変更したい場合は **inout** 引数を使います。
~~~
// 値を入れ替える関数
func changeValue(inout a:Int, inout _ b:Int)
{
    let temp = a
    a = b
    b = temp
}

var x = 100
var y = 20
changeValue(&x, &y)
print("x = \(x), y = \(y)") // x = 20, y = 100
~~~
changeValue関数は引数で渡された値を入れ替えています。
関数の処理によって呼び出し側の変数を変更したい場合、引数にinoutというキーワード
を付加します。外部引数名または'_'を指定する場合は、その前に置きます。

#### 引数の規定値
C言語のデフォルト引数と同様に、Swiftでも関数の引数に規定値を付けることができます。
規定値には定数やグローバル変数の値を設定できます。
引数に規定値を付ける例を下記に示します。
~~~
let fontSize: Double = 12.0

func setFont(name: String, size: Double = fontSize, bold: Bool = false)
{
    print("\(name) \(size)" + (bold ? " [B]" : ""))
}

setFont("RanglanPunch")                     // RanglanPunch 12.0
setFont("Courier", bold: true)              // Clurier 12.0 [B]
setFont("Times", size: 16.0, bold: false)   // Times 16.0
setFont("Times", bold: true, size: 18.0)    // Times 18.0 [B]


func setGray(level: Int = 255, _ alpha: Double = 1.0)
{
    print("Gray = \(level), Alpha = \(alpha)")
}

setGray()           // Gray = 255, Alpha = 1.0
setGray(240)        // Gray = 240, Alpha = 1.0
setGray(128, 0.5)   // Gray = 128, Alpha = 0.5
~~~
規定値を使用した引数のルールは次の1〜3です。
1. 規定値を持つ引数は引数リストのどこにあっても構いません。
ただし、その引数よりも後ろ(右)の規定値を持たない引数には外部引数名を付ける
必要があります。でないと、規定値を持つ引数も呼び出し時に省略できなくなります。
 * 例のsetFont("Courier", bold: true) では第3引数に外部引数名(bold)を付けているため
第2引数のsizeを省略することができています。
2. 外部引数名を持つ複数の引数に規定値が指定されている場合、
指定する必要のある実引数だけ記述することができます。また引数リストとは違う順序で記述することができます。
ただし、規定値のない引数と順番が前後してはいけません。
 * 例のsetFont("Times", bold: true, size: 18.0)では引数の順序が第2と第3とで入れ替わっています。
3. 外部引数名を持たない引数に規定値が指定されている場合、ある引数から後ろ(右)をまとめて省略
することができますが、途中の引数だけ省略することはできません。
 * 例のsetGray関数で、setGray(240)の場合は第2引数を省略できますが、第1引数だけを省略
することはできません。

#### 引数の値を処理中に変更できるようにする
関数の引数はデフォルトで定数となり、関数内の処理で引数の値を返すことはできません。
inout引数は変更できますが、Swiftで単純に引数の値を変えたいだけの場合は引数名の前に
**var** を付けることで変更可能になります。また、変更したくないと明示的に示したい場合は
**let** を付けることもできます。let, var, inoutの指定を同時に行うことはできません。

#### 返り値を必ず使うための指定
SwiftはC言語と同様に、返り値があっても後の処理に必ずしも使う必要はありません。
しかし、引数を渡して計算し結果を返すなどの関数については、返り値を捨ててしまうのは間違いだと考えられます。
このような場合、Swiftでは返り値が使われるように警告を出すための **@warn_unused_result**
という属性(attribute)を指定することができます。
~~~
@warn_unused_result
func add(a: Int, b: Int) -> Int
{
    return a + b
}

add(10, b: 20)		// warnning "Result of call to 'add(_:b:) is unused'"
let result = add(10, b: 20)   // 警告はでない
print(add(10, b: 20))         // 警告はでない
~~~

#### 関数内の関数
Swiftでは関数内に関数を作成することができます。関数内で作成した関数は外部から参照することはでき
ません。関数の作り方は基本的に同じです。

#### 関数のオーバーロード
SwiftでもC++やJavaと同様にオーバーロードの機能があります。
引数が違う関数や戻り値の型が違う関数を作成することができます。

引数の数、型が一緒の場合でも外部引数名が違う場合もオーバーロードすることができます。
~~~
// 引数の値を入れ替える関数
func changeValue(inout a: Int, inout _ b: Int)
{
	let temp = a
	a = b
	b = temp
}

// 引数の値を入れ替える関数
// ただし、aの値がbの値より小さい場合は入れ替えない
func changeValue(inout little a: Int, inout great b: Int)
{
	if a < b
	{
		let temp = a
		a = b
		b = temp
	}
}
~~~
