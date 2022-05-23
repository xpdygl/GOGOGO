## Panic 异常

panic会引起程序的崩溃，因此panic一般用于严重错误，如程序内部的逻辑不一致。

当panic异常发生时，程序会中断运行，并立即执行在该goroutine中被延迟的函数（defer 机制）。随后，程序崩溃并输出日志信息





## Recover捕获异常

recover被定义在defer之中



deferred函数中调用了内置函数recover，并且定义该defer语句的函数发生了panic异常，recover会使程序从panic中恢复，并返回panic value。导致panic异常的函数不会继续运行，但能正常返回。在未发生panic时调用recover，recover会返回nil。