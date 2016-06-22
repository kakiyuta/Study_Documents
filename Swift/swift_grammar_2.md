## swift文法2

### 関数
---
swiftで関数を作成すると下記のようになります。
~~~
func greet(name: String, day: String) -> String {
    return "Hello \(name), today is \(day)."
}

print(greet("Bob", day: "Tuesday"))     // Hello Bob, today is Tuesday
~~~
関数の戻り値の型は **->** で指定する。

swiftでは、関数の呼び出し側で引数の名前を指定する必要があります。
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
関数area()には引数が二つあります。それぞれにheight, widthと外部引数名を付けています。
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
