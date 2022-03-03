# 1 What is Concurrent

## 1.1 给并发建模

```erl
%% person.erl

-module(person).
-export([init/1]).

init(Name) -> ...
```

`-module(person).` 表示此文件包含用于模块`person`的代码。  
它应该与文件名一致。模块名必须以小写字母开头(模块名是一个*atom*)

接着是一条导出声明。导出声明指定了模块里哪些函数可以从模块外部进行调用。  
没有包括在导出声明里的函数是私有的，无法在模块外调用。  
导出的多个函数用逗号分隔。

```
%% world.erl

-module(world).
-export([start/0]).

start() ->
    Joe = spawn(person, init, ["Joe"]),
    Dave = spawn(person, init, ["Dave"]).
    % ...
```

`spawn` 是一个Erlang基本函数，它会创建一个并发进程并返回一个进程标识符。  
`spawn`可以这样调用：  
`spawn(ModName, FuncName, [Arg1, Arg2 ...]`  


