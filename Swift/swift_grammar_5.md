## Swift文法5

### 整数と実数
---

Swiftでの数字の表し方には、C言語と異なる点が複数存在します。

10進数について、大きな数字を表す際に桁数が分かりやすくするため、
数字と数字の間に **「_」** を入れることができます。
入れる位置は先頭以外ならどこでも構いません。
またC言語では先頭が0の場合、8進数として扱われますが、そのような決まりはSwiftにはありません。
下記の数字は全て同じ「123456789」を表しています。
~~~
123456789,  0123456789,  0_123_456_789,  1234_56_789
~~~

16進数は先頭が **「0x」** と記述することで扱えます。
xは小文字でなければなりません。
A〜F, a〜fのいずれを使っても構いません。
~~~
0x1234abc,  0x12_34_abc,  0x123_4A_B_cd
~~~

8進数の場合は先頭が **「0o」** と記述することで扱えます。
oは小文字でなけばなりません。
~~~
0o12345670,  0o12_34_567
~~~

2進数の場合は先頭が **「0b」** と記述することで扱えます。
bは小文字でなければなりません。
~~~
0b001101011,   0b0011_1101_1100
~~~

### 配列
---

Swift文法1でも配列について説明しましたが、ここではより詳しく配列の性質について説明します。

#### 配列の比較
Swiftでは配列同士で比較ができます。使用できる演算子は「==」,「!=」のみです。
~~~
let a = [1, 2]
let b = [2, 1]
a == b					// false
a + [1] == [1] + b		// true      [1, 2, 1]で構成されるため
~~~

#### 部分配列
配列の添字に整数型の範囲を指定することによって、
その添字の範囲の要素からなる新しい配列を作ることができます。
~~~
var gonben = ["語", "誠", "設", "言", "論", "調", "課"]
var sub = gonben[1..<4]
print(sub)			// ["誠", "設", "言"] を出力
~~~

また添字で範囲を指定し、その範囲の値を書き換えることもできます。
~~~
var s = ["春", "夏", "秋", "冬"]
s[0...0] = ["初夏", "梅雨"]		// s[0]ではだめ
print(s)		// ["初夏", "梅雨", "夏", "秋", "冬"]
s[1...3] = ["花見", "夏休み", "盆"]
print(s)		// ["初夏", "花見", "夏休み", "盆", "冬"]
s[3...4] = []	// 3〜4を削除
print(s)		// // ["初夏", "花見", "夏休み"]
~~~

#### 部分配列の型と添字
