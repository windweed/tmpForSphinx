# 3 Basic Concepts

无论是并行还是顺序，所有的Erlang程序都会用到模式匹配、一次性变量赋值、用于表示数据的基本类型。

## 3.1 Erlang shell

Unix系统从命令行启动Erlang shell, Windows系统从开始菜单的图标启动。

停止Erlang shell的方式是`Ctrl + C`, Windows系统是`Ctrl + Break`, 然后按`a`。  
输入`a`会立即停止系统，这可能导致数据损坏。要实现受控关闭，使用`q().`。  
`q()`是`init:stop()`在shell里的别名。  
要立即停止系统，使用`erlang:halt().`

Erlang用`%`注释。  

Erlang模块里的语法形式不是表达式，比如附注(`-`开头的事物，比如`-module`,`-export`)。  
它们不能被shell理解。

Erlang shell 快捷键(`^Key` 即 `Ctrl + Key`)
|命令|说明|
|:-:|:-:|
|^A|行首|
|^D|删除当前字符|
|^E|行尾|
|^F|向右|
|^B|向左|
|^P|上此输入|
|^N|下次输入|
|^T|调换左边最近两个字符的位置|

shell的强大之处在于，可以挂接一个shell到集群里另一个Erlang节点上运行的Erlang系统，甚至还  
可以生成一个SSH直接连接远程计算机上运行的Erlang系统。  
通过它，可以与Erlang节点系统中任何节点上的任何程序进行交互。

## 3.2 Integer

```sh
1> 2 + 3 * 4.
14
2> (2 + 3) * 4.
20
```

Erlang可以用任意长度的整数执行整数运算。在Erlang，整数运算是精确的，因此，  
无需担心溢出，但也无法用特定字长表示某个整数。
```sh
3> 123456789 * 987654321 * 112233445566778899 * 998877665544332211.
13669560260321809985966198898925761696613427909935341
```
任意进制整数：
```sh
4> 16#cafe * 32#sugar.
1577682511434
```

## 3.3 Variable

```sh
1> X = 123456789.
123456789

```
**所有变量名必须以大写字母开头**

Erlang的变量是**一次性赋值变量**(single-assignment variable)。  
已被指定一个值的变量称为*绑定变量*，否则称为未绑定变量**。

变量的作用域就是它定义时所处的语汇单元。  

在Erlang里，`=`是一次*模式匹配操作*。`L = R` 的真正意思是：计算`R`的值，然后将结果与`L`  
的模式相匹配。  

变量是模式的一种简单形式。

## 3.4 Float

```sh
5> 5 / 3.   % / 将结果自动转换为浮点数
1.6666666666666667
6> 4 / 2.
2.0
7> 5 div 3.  % 整除
1
8> 5 rem 3.  % 取余
2
```
Erlang在内部使用64位的IEEE754-1985浮点数，因此使用浮点数的程序会存在和C语言一样的浮点数  
取整与精度问题。

## 3.5 Atom

Erlang中，原子被用于表示常量值。类似于C的宏/枚举。  

在Erlang里，原子是全局性的，而且不需要宏定义或包含文件就能实现。  

原子以小写字母开头，后接数字、字母、下划线或`@`。  

原子还可以放在单引号内，可以以这种形式创建大写字母开头的或包含其他字符的原子。如  
`'Monday'`、`'Tuesday abc'`、`'+'`。  
甚至还可以给无需引号的原子加上引号，所以`'a'`和`a`的意思完全一致。  
双引号用于字符串字面量。  

一个原子的值就是它本身：
```sh
9> a.
a

```
Erlang是一种函数式语言，每个表达式都必须有一个值。

## 3.6 Tuple

元组用于把一些数量固定的项目归组成单一的实体。使用大括号创建，逗号分隔。  

元组里的字段没有名字。因此为了更容易记住字段的用途，一种常用的做法是将原子作为元组的第一个元素，  
用它来表示元组是什么。如，创建一个坐标：`P = {point, 10, 23}.`。

元组可以嵌套：
```sh
10> Person = {person, {name, joe}, {height, 1.82}, {footsize, 42}, {eyecolour, brown}}.
{person,{name,joe},
        {height,1.82},
        {footsize,42},
        {eyecolour,brown}}

```

元组在声明时自动创建，不再使用时自动销毁。  
如果创建时用到变量，新的元组会共享该变量所引用数据结构的值。

```sh
11> F = {firstName, joe}.
{firstName,joe}
12> L = {lastName, armstrong}.
{lastName,armstrong}
13> P = {person, F, L}.
{person,{firstName,joe},{lastName,armstrong}}

14> {true, Q, 23, Costs}.   % 引用未定义的变量
* 1:8: variable 'Q' is unbound

```

**提取元组的值**

```sh
1> P = {point, 1, 2}.
{point,1,2}
2> {point, X, Y} = P.
{point,1,2}
3> X.
1
4> Y.
2
5> P1 = {point, 25, 25}.
{point,25,25}
6> {point, C, C} = P1.
{point,25,25}
7> C.
25
8> Person = {person, {name, joe, armstrong}, {footsize, 42}}.
{person,{name,joe,armstrong},{footsize,42}}
9> {_, {_, Who, _}, _} = Person.
{person,{name,joe,armstrong},{footsize,42}}
10> Who.
joe

```

## 3.7 List

列表用来存放任意数量的事物。使用中括号创建，逗号分隔。  

各元素可以是任何类型：
```sh
11> [1 + 7, hello, 2 - 2, {cost, apple, 30 - 20}, 3].
[8,hello,0,{cost,apple,10},3]

```

列表的第一个元素被称为列表头(head)。假设去掉列表头，剩下的就称为列表尾(tail)。  
如,`[1,2,3,4,5]`的列表头就是`1`，列表尾则是列表`[2,3,4,5]`。

访问列表头是一种非常高效的操作，因此基本所有的列表处理函数都从提取列表头开始，进行一些操作，  
接着处理列表尾。  

如果`T`是一个列表，那么`[H|T]`也是一个列表，它的头是`H`, 尾是`T`。`|`把列表的头尾分隔开。  
`[]`是一个空列表。  

无论何时，只要用`[...|T]`语法构建一个列表，就应该确保`T`是列表。如果它是，那么新列表就是  
*格式正确的*，如果`T`不是列表，那么新列表就是*不正确的列表*。

可以给`T`的开头添加不止一个元素，写法是`[E1, E2, ..., En|T]`。
e.g.:
```sh
12> ThingsToBuy = [{apples, 10}, {pears, 6}].
[{apples,10},{pears,6}]
13> ThingsToBuy1 = [{orangs, 4}, {newspaper, 1}|ThingsToBuy]. % 扩展
[{orangs,4},{newspaper,1},{apples,10},{pears,6}]

```

**提取列表的值**

`[X|Y] = L.` 会提取列表头作为`X`，列表尾作为`Y`。

```sh
14> [Buy1|ThingsToBuy2] = ThingsToBuy1.
[{orangs,4},{newspaper,1},{apples,10},{pears,6}]
15> Buy1.
{orangs,4}
16> ThingsToBuy2.
[{newspaper,1},{apples,10},{pears,6}]
17> [Buy2, Buy3|ThingsToBuy3] = ThingsToBuy2.
[{newspaper,1},{apples,10},{pears,6}]
18> Buy2.
{newspaper,1}
19> Buy3.
{apples,10}
20> ThingsToBuy3.
[{pears,6}]

```

## 3.8 String

严格来说，Erlang没有字符串，要在Erlang里表示字符串，可以选择一个由整数组成的列表或者一个  
二进制型(7.1节)。  
当字符串表示为一个整数列表时，列表时每个元素都代表一个Unicode代码点(codepoint)。

可以用字符串字面量来创建一个列表。  
```sh
21> Name = "Hello".
"Hello"

```
如果所有整数都代表可打印字符，就会打印成字符串字面量。  
```sh
22> [83, 117, 114, 112, 114, 105, 115, 101].
"Surprise"
23> [1, 83, 117, 114, 112, 114, 105, 115, 101].
[1,83,117,114,112,114,105,115,101]

```

`$a`: 代表字符`a`的整数。
```sh
25> [I - 32, $u, $r, $p, $r, $i, $s, $e].
"Surprise"

```
用列表表示字符串时，里面的整数都代表Unicode字符。  
```sh
27> Z = "a\x{221e}b".
[97,8734,98]
28> io:format("~ts~n", [Z]).
a∞b

```

如果想让shell将整数列表打印成整数而非字符串，需要格式化打印：
```sh
30> io:format("~w~n", ["abc"]).
[97,98,99]
ok

```

# 3.9 模式匹配再探

单位(term)就是指任何一种Erlang数据结构。  

阅读以下例子

|pattern = term|result|
|---|----|
|`{X, abc} = {123, abc}`|succ: X = 123|
|`{X, Y} = {333, ghi, "cat"}`|fail: 元组形状不同|
|`{X, Y, X} = {{abc, 12}, 2, {abc, 12}}`|succ: X = {abc, 12}, Y = 2|
|`[H|T] = "cat"`|succ: H = 99, T = "at"|
|`[A,B,C|T] = [a,b,c,d,e,f]`|succ: A=a, B=b, C=c, T=[d,e,f]|

```sh
1> X = 1.
1
2> f().
ok
3> X = 2.
2
```
`f()`让shell忘记现有的所有绑定。
