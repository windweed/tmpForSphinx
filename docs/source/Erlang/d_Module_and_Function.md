# 4 Module and Function

模块和函数时构建顺序与并行程序的基本单元。  
模块包含了函数，而函数可以顺序或并行运行。

## 4.1

模块是Erlang的基本代码单元。  
模块保存在扩展名为`.erl`的文件里，必须先编译才能运行。编译后的模块以`.beam`作为扩展名。  

下面给出一个名为`area`的函数，它计算正方形/长方形的面积。

```erl
%% geometry.erl
-module(geometry).  %% 模块声明。声明里的模块名必须与存放该模块的主文件名相同
-export([area/1]). %% 导出声明。Name/N 代表一个带有N个参数的函数Name，N称为元数(arity)

area({rectangle, Width, Height}) -> Width * Height; %% 分号隔开子句， ->隔开子句的头部和主体
area({square, Side}) -> Side * Side.
```
子句会根据它们在函数定义里出现的顺序进行匹配。  
函数不需要显式的返回语句，返回值就是子句主体里最后一条表达式的值。

```erl
8> c(geometry).
{ok,geometry}
9> geometry:area({rectangle, 10, 5}).
50
10> geometry:area({square, 10}).
100
11>
```

Erlang shell 部分内建命令：
* `pwd()` 打印当前工作目录
* `ls()` 列出当前工作目录的所有文件名
* `cd(Dir)` 修改当前工作目录至Dir

**给代码添加测试**

可以给模块添加一些简单的测试。
```erl
%% geometry.erl

test() ->
    12 = area({rectangle, 3, 4}),
    100 = area({square, 10}),
    test_worked.
```

```erl
12> geometry:test().
test_worked

```

函数并不处理模式匹配失败的情形，程序会以一个运行时错误结束。  
这是故意为之的，是在Erlang里编程的方式。

## 4.2 例子：购物
