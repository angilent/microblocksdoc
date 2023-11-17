- 有关 MicroBlocks VM 的技术信息
## 指示

MicroBlocks 代码由运行在微控制器板上的简单解释器或虚拟机 (VM) 执行。这个虚拟机的指令是 32 位的，由一个 8 位操作（操作码）和一个 24 位操作数组成：

```
<operand (24 bits)><opcode (8-bits)>
```

复制

操作码告诉虚拟机要做什么。不同的操作码以不同的方式使用操作数。例如，它可用于对常量值或跳跃偏移进行编码。一些操作码根本不使用操作数，但是解释器和编译器都通过对所有操作码使用相同的指令格式进行了简化。

解释器是一个堆栈机器。要执行给定的操作码，其参数（即任何参数的值）首先被压入堆栈，然后运行操作码的代码。该代码将所有参数从堆栈中弹出，并且可以选择将返回值压入堆栈。

例如，指令`set user LED <true>`是：

```
pushImmediate true    // push the boolean value 'true'
setLEDOp      // pop 'true' off the stack and set the state of the user LED to it
```

复制

的说明`set user LED (tilt-x < 10)`是：

```
mbTiltX        // pushes the value of the tilt X sensor
pushImmediate 10    // pushes the number 10
lessThan      // remove the two arguments and pushes true if tilt-x is less than 10
setLEDOp      // pop a boolean value and and set the state of the user LED to it
```

复制
## 任务

MicroBlocks 似乎同时运行许多脚本，但这是一种错觉。虚拟机实际上运行一个脚本的几条指令，然后运行下一个脚本的几条指令，依此类推。它一次只执行一个脚本，但在这些脚本（或“任务”）之间切换的速度如此之快，以至于它们看起来像是在同时运行。

与许多编程语言不同，MicroBlocks 只能在代码中某些明确定义的点切换到下一个任务：在循环结束时或在显式等待的指令处，例如`wait 10 milliseconds`. 这避免了在其他语言中遇到的许多并发问题（或竞争条件）。这也意味着高级用户可以在 MicroBlocks 中实现他们自己的更高级别的并发机制（例如锁、信号量或监视器），因为测试之后的某些操作（例如设置标志）不能被另一个 MicroBlocks 任务中途中断。这种不可中断的序列有时称为临界区。在 MicroBlocks 中，每个不包含循环或显式等待的块的命令序列都是隐式临界区。

在运行用户脚本之间，MicroBlocks 虚拟机还负责处理系统事务。例如，它会在电路板连接时检查来自编程环境的传入命令。在某些设备上，它还执行额外的系统任务。例如，在 BBC micro:bit 上，虚拟机会定期更新 5x5 LED 显示屏。
## 协议

MicroBlocks VM 通过简单的串行协议与 IDE 和其他类型的软件（例如 Mozilla Web of Things 网关）通信。

[您可以在此处](https://bitbucket.org/john_maloney/smallvm/src/master/misc/SERIAL_PROTOCOL.md)找到此协议的规范。