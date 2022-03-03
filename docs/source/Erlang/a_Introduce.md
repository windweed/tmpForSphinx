# 0 preface

编写Erlang程序时，实现的方式不是让单个进程执行所有任务，而是生成大量只做简单事情的小进程，  
并让它们互相通信。

```erl
% shop1.erl
-module(shop1).
-export([total/1]).

total([{What, N} | T]) -> shop:cost(What) * N + total(T);
total([]) -> 0.

```
