system call

`layout src` 展示调试时的源代码
(类似的命令)
`backstrace` 打印栈回溯 (stack backtrace)

### gdb 使用 tips
`p/t`  以二进制形式
`p/x`  以 0x 形式
`p/d` 以 decimal 形式 (十进制)

### sstatus
关于 `$sstatus` 寄存器的内容
`Supervisor Status Register`
$sstatus$ 是 $RISC-V$ 架构中的一个特殊寄存器，它包含了一些系统级别的状态信息和控制位。下面是 $sstatus$ 寄存器的一些相关信息：

1. $Supervisor Status (SSTATUS) Register$ : $\$sstatus$ 寄存器是一个 64 位的寄存器，在 RISC-V 架构中用于存储和控制处理器的系统级别状态。

2. 状态位（Status Fields）：$\$sstatus$ 寄存器的不同位域用于表示不同的状态信息，其中一些重要的位域包括：

   - $UIE（User Interrupt Enable）$: 用户中断使能位，控制用户模式下是否允许中断。
   - $SIE（Supervisor Interrupt Enable）$: 监管者中断使能位，控制监管者模式下是否允许中断。
   - $MIE（Machine Interrupt Enable）$: 机器中断使能位，控制机器模式下是否允许中断。
   - $UPIE（User Previous Interrupt Enable）$: 用户模式下的上一个中断使能位，保存了上一个中断发生时 UIE 的值，用于中断返回时恢复状态。
   - $SPIE（Supervisor Previous Interrupt Enable）$: 监管者模式下的上一个中断使能位，保存了上一个中断发生时 SIE 的值，用于中断返回时恢复状态。
   - $MPIE（Machine Previous Interrupt Enable）$: 机器模式下的上一个中断使能位，保存了上一个中断发生时 MIE 的值，用于中断返回时恢复状态。
   - $SPP（Supervisor Previous Privilege）$: 上一个特权级位，表示上一个中断或异常发生时的特权级。
   - $SIE（Supervisor Interrupt Enable）$: 监管者中断使能位，控制监管者模式下是否允许中断。
   - $SPIE（Supervisor Previous Interrupt Enable）$ : 监管者模式下的上一个中断使能位，保存了上一个中断发生时 SIE 的值，用于中断返回时恢复状态。
   - $SPP（Supervisor Previous Privilege）$: 上一个特权级位，表示上一个中断或异常发生时的特权级。

3. 特权级控制（Privilege Control）：$sstatus 寄存器中的一些位域用于控制特权级别切换和处理器模式的转换。

   - SPP（Supervisor Previous Privilege）: 上一个特权级位，表示上一个中断或异常发生时的特权级。
   - SIE（Supervisor Interrupt Enable）: 监管者中断使能位，控制监管者模式下是否允许中断。
   - SPIE（Supervisor Previous Interrupt Enable）: 监管者模式下的上一个中断使能位，保存了上一个中断发生时 SIE 的值，用于中断返回时恢复状态。

4.  中断控制（Interrupt Control）：$sstatus 寄存器中的中断使能位用于控制处理器是否允许中断的发生。这些位域包括：
    
    -   UIE（User Interrupt Enable）: 用户中断使能位，控制用户模式下是否允许中断。
    -   SIE（Supervisor Interrupt Enable）: 监管者中断使能位，控制监管者模式下是否允许中断。
    -   MIE（Machine Interrupt Enable）: 机器中断使能位，控制机器模式下是否允许中断。
    
    这些中断使能位可以通过设置为1来允许中断，或设置为0来禁止中断。当中断发生时，处理器会根据特权级别和中断使能位来确定是否处理该中断。
    

需要注意的是，$sstatus寄存器只能在特权级别高于等于S级别（Supervisor）时才能访问和修改。在特权级别为U级别（User）时，访问和修改$sstatus寄存器会引发异常。

以上是关于$sstatus 寄存器的一些相关信息，希望对你的学习有所帮助。如果你还有其他问题，请随时提问。

### scause
下面是关于$scause 寄存器的一些要点：

1.  寄存器描述：$scause是一个64位的寄存器，存储着导致最近异常的异常原因代码。
    
2.  异常原因代码：异常原因代码用于标识导致异常的具体原因。它是一个整数，每个异常类型都有一个唯一的异常原因代码。这些代码在RISC-V架构规范中定义，并且在不同的实现中可能会有所不同。
    
3.  异常分类：异常原因代码分为三个主要类别：
    
    -   中断（Interrupts）：如外部中断、定时器中断等。
    -   故障（Faults）：如页面错误、访问非法指令等。
    -   陷阱（Traps）：如系统调用、断点指令等。
4.  编码格式：$scause寄存器中的异常原因代码的编码格式是由M位的"Interrupt"位和N位的"Exception Code"位组成，其中M + N = 64。"Interrupt"位用于区分中断和异常，当其为0时表示异常，为1时表示中断。"Exception Code"位用于表示具体的异常类型。
    
5.  异常返回地址：在异常处理程序中，可以使用$scause寄存器的值来确定如何处理异常。异常处理程序可以根据异常原因代码决定如何恢复现场，并选择相应的操作，如重新执行指令、返回用户程序或转移到特定的异常处理流程。
    

需要注意的是，$scause寄存器只能在特权级别高于等于S级别（Supervisor）时才能访问和修改。在特权级别为U级别（User）时，访问和修改$scause寄存器会引发异常。

这些是关于$scause 寄存器的一些基本信息。希望这对你有所帮助！如果你还有其他问题，请随时提问。

### Sepc
$sepc（Supervisor Exception Program Counter)$ 是 $RISC-V$ 架构中的一个特殊寄存器，用于存储异常处理程序的地址。当异常发生时，处理器会将异常处理程序的地址加载到 sepc 寄存器中，以便在异常处理过程中能够正确地返回到异常发生的指令之后。

以下是关于$sepc 寄存器的一些要点：

1. 寄存器描述：$sepc 是一个 64 位的寄存器，用于存储异常处理程序的地址。

2. 异常处理程序：异常处理程序是一段特殊的代码，用于处理异常情况。当异常发生时，处理器会根据异常原因代码和特权级别确定要执行的异常处理程序。

3. 异常返回地址：在异常处理程序执行完毕后，处理器需要返回到异常发生的指令之后继续执行。为了实现这一点，处理器将异常发生时的指令地址保存在$sepc 寄存器中，以便在异常处理程序执行完毕后，将该地址加载到程序计数器（PC）中。

4. 异常返回指令：处理器在异常处理程序的结尾通常会使用一个特殊的指令，称为异常返回指令。这个指令会从$sepc 寄存器中读取保存的地址，并将其加载到 PC 中，从而实现从异常处理程序返回到异常发生的指令之后继续执行。

需要注意的是，$sepc寄存器只能在特权级别高于等于S级别（Supervisor）时才能访问和修改。在特权级别为U级别（User）时，访问和修改$ sepc 寄存器会引发异常。

在异常处理过程中，程序可以读取和修改$sepc 寄存器的值，以便在需要时调整异常返回的位置或进行其他操作。

这些是关于$sepc 寄存器的一些基本信息。希望对你的学习有所帮助！如果你还有其他问题，请随时提问。
### 小错误
``` bash
riscv64-linux-gnu-gcc    -c -o kernel/entry.o kernel/entry.S
make: riscv64-linux-gnu-gcc: Command not found
make: *** [<builtin>: kernel/entry.o] Error 127
```

根据错误提示，看起来你的系统上缺少riscv64-linux-gnu-gcc编译器或者该编译器没有
正确配置到系统的环境变量中。这是导致make命令失败的原因。

要解决这个问题，你可以尝试以下步骤：

1.  确认是否已安装riscv64-linux-gnu-gcc编译器：检查你的系统中是否已经安装了RISC-V架构的交叉编译工具链，特别是riscv64-linux-gnu-gcc编译器。你可以通过运行以下命令来确认是否已安装：
    
    `riscv64-linux-gnu-gcc --version`
    
    如果命令执行成功并显示了 gcc 的版本信息，则说明编译器已正确安装

机组的东西有一些是没整明白的

## 4.1 $RISC-V$ Trap mechanism 
每个 $risc-v\quad cpu$ 都有一组控制寄存器 (`Control and Status Registers`)
