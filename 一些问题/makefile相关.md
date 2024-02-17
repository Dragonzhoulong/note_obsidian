```bash
LIBS += $(if $(CONFIG_TARGET_NATIVE_ELF),-lreadline -ldl -pie,)
```

### $
>注重 $ 的作用

在 `Makefile` 中，`$` 符号有特殊的含义，它通常用于表示变量引用、命令替换和函数调用等。以下是 `Makefile` 中 `$` 符号的几种常见用法：

1. **变量引用：** 在`Makefile`中，我们可以使用变量来保存和管理数据。使用`$`符号可以引用这些变量的值。例如，如果有一个变量`CC`，它表示编译器的名称，我们可以使用`$(CC)`来引用这个变量，如：`$(CC) file.c -o output`。
    
2. **命令替换：** 通过使用反引号（`）`或`$()`，可以在`Makefile`中执行命令，并将其输出作为一个变量的值。例如，可以通过`$(shell command)`来执行命令并将输出保存到一个变量中，如：`FILES = $(shell ls)`，这将把当前目录下的文件列表保存在变量`FILES`中。
    
3. **自动化变量：** 在规则中，`$@`表示目标文件的名称，`$<`表示第一个依赖文件的名称，`$^`表示所有依赖文件的列表。例如，下面的规则将会把`file.c`编译为`file.o`：`file.o: file.c`，`$(CC) -c $< -o $@`。
    
4. **函数调用：** `Makefile`中有一些内置的函数，可以使用`$()`语法来调用这些函数。例如，`$(wildcard pattern)`函数可以用来匹配文件路径模式，返回匹配的文件列表。
    
5. **转义字符：** 如果想要在`Makefile`中使用`$`符号本身而不是其特殊含义，可以通过`$$`进行转义。例如，`$$`将会被解释为单个`$`字符。
    

---------------
### LIBS 的作用域
implict rule 的作用?


















-------------
#### $代码讲解
请注意，`Makefile`中的`$`符号的含义可能会因不同的`Makefile`解释器（如GNU Make、BSD Make等）以及版本而有所不同，因此在编写`Makefile`时要小心处理`$`符号的使用，以确保得到正确的结果。

这是一个 `Makefile` 中的变量定义，它的含义如下：

1. `LIBS +=`: 这表示将后面的内容添加到名为 `LIBS` 的变量中。`+=` 运算符是用来追加内容而不是替换。
    
2. `$(if $(CONFIG_TARGET_NATIVE_ELF), ... , ...)`: 这是一个条件表达式。它检查名为 `CONFIG_TARGET_NATIVE_ELF` 的变量是否已定义且非空。如果 `CONFIG_TARGET_NATIVE_ELF` 已定义且非空，它会执行第一个逗号前的内容；否则，执行第一个逗号后的内容。
    
3. `-lreadline -ldl -pie,`: 这是条件表达式的两个分支之一。如果 `CONFIG_TARGET_NATIVE_ELF` 已定义且非空，那么将会添加 `-lreadline -ldl -pie` 到 `LIBS` 变量中。
    

让我们解释一下：

假设 `CONFIG_TARGET_NATIVE_ELF` 已经定义且非空，那么 `LIBS` 变量将被扩展为 `$(LIBS) -lreadline -ldl -pie`，也就是在原来 `LIBS` 变量的基础上，追加了 `-lreadline -ldl -pie` 这些内容。

这里的目的是根据条件是否为原生 ELF 目标来选择性地链接 `readline` (`-lreadline`)、动态链接器 (`-ldl`) 以及生成位置无关代码 (`-pie`) 到可执行文件中。这样可以根据编译目标的不同来添加不同的链接选项，以确保生成符合目标平台的可执行文件。

请注意，该段代码只是一个片段，可能在实际的 `Makefile` 文件中有更多的内容和逻辑。要正确理解这段代码的作用，需要了解 `Makefile` 文件的完整内容和相关的变量定
义。


1. `TOPNAME = top` : 定义了顶层模块的名称为"top"。
    
2. `NXDC_FILES = constr/top.nxdc`: 定义了约束文件的路径为"constr/top.nxdc"。
    
3. `VERILATOR = verilator`: 定义了Verilator仿真工具的名称为"verilator"。
    
4. `VERILATOR_CFLAGS`: 定义了Verilator编译选项，包括一些优化选项和其他标志。
    
5. `BUILD_DIR = ./build`和`OBJ_DIR = $(BUILD_DIR)/obj_dir`: 定义了构建目录和对象目录的路径。
    
6. `BIN = $(BUILD_DIR)/$(TOPNAME)`: 定义了生成的可执行文件的路径和名称。
    
7. `default: $(BIN)`: 默认目标是构建可执行文件。
    
8. `$(shell mkdir -p $(BUILD_DIR))`: 使用`mkdir -p`命令在构建目录创建子目录。
    
9. `SRC_AUTO_BIND`: 定义了一个自动生成绑定源文件的规则。
    
10. `VSRCS`和`CSRCS`: 分别定义了Verilog和C/C++源文件的路径。
    
11. `include $(NVBOARD_HOME)/scripts/nvboard.mk`: 包含了"nvboard.mk"文件，可能是用于NVBoard项目的构建规则。
    
12. `INCFLAGS`、`CFLAGS`和`LDFLAGS`: 分别定义了头文件路径、C/C++编译选项和链接选项。
    
13. `$(BIN): $(VSRCS) $(CSRCS) $(NVBOARD_ARCHIVE)`: 定义了可执行文件的依赖关系。
    
14. `all: default`: 定义了"all"目标，它依赖于"default"目标。
    
15. `run: $(BIN)`: 定义了"run"目标，用于运行生成的可执行文件。
    
16. `clean`: 定义了"clean"目标，用于删除生成的构建目录和对象文件。
    
17. `.PHONY: default all clean run` : 声明了一些伪目标，告诉 Makefile 这些目标不代表真实的文件。

### makefile 详细过程

https://ysyx.oscc.cc/slides/2205/06.html#/%E5%9B%9E%E9%A1%BE-%E4%BA%86%E8%A7%A3%E7%A8%8B%E5%BA%8F%E5%B7%A5%E5%85%B7%E8%A1%8C%E4%B8%BA%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%96%B9%E6%B3%95/1
```
make –trace
```
可以看到 make 的实际执行中的头文件寻找方法
```
gcc -O2 -MMD -Wall -Werror -I/home/along/ics2021/nemu/include -I/home/along/ics2021/nemu/src/engine/interpreter -I/home/along/ics2021/nemu/src/isa/riscv64/include -O2  -ggdb3 -fsanitize=address -DITRACE_COND=true -D__GUEST_ISA__=riscv64 -c -o /home/along/ics2021/nemu/build/obj-riscv64-nemu-interpreter/src/cpu/cpu-exec.o src/cpu/cpu-exec.c
```

一开始我想将在 `cpu-exec. c` 中调取 `src/sdb/sdb. c` 中的函数, (果然 `src/sdb/sdb. h` 不在 ` cpu-exec. c` 被 makefile 编译的路径当中), 我再来酷酷酷

```
gcc -O2 -MMD -Wall -Werror 
-I/home/along/ics2021/nemu/include 
-I/home/along/ics2021/nemu/src/engine/interpreter 
-I/home/along/ics2021/nemu/src/isa/riscv64/include 
-O2  -ggdb3 -fsanitize=address -DITRACE_COND=true -D__GUEST_ISA__=riscv64 -c -o /home/along/ics2021/nemu/build/obj-riscv64-nemu-interpreter/src/monitor/sdb/sdb.o src/monitor/sdb/sdb.c
```
所以 sdb. c 中倒是可以很顺利的调用 include 中的头文件

`watchpoint.c` 内引用 `cpu/cpu.h`
`cpu-exec.c` 内引用 `cpu/cpu.h`
Watch 


#### 输出结果

```c
(nemu) info w
No watchpoints
(nemu) w 0x100000
[nemu/src/monitor/debug/expr.c,98,make_token] match rules[6] = "0[xX][0-9a-fA-F]+" at position 0 with len 8: 0x100000
Watch point 0: 0x100000
(nemu) si
  100000:   bd 00 00 00 00                        movl $0x0,%ebp
[nemu/src/monitor/debug/expr.c,98,make_token] match rules[6] = "0[xX][0-9a-fA-F]+" at position 0 with len 8: 0x100000
Watchpoint 0: 0x100000
Old value = 0
New value = 0
(nemu) w 0x888888
[nemu/src/monitor/debug/expr.c,98,make_token] match rules[6] = "0[xX][0-9a-fA-F]+" at position 0 with len 8: 0x888888
Watch point 1: 0x888888
(nemu) info w
Watch point 0: 0x100000
Watch point 1: 0x888888
(nemu) d 0
Delete watchpoint 0: 0x100000
(nemu) info w
Watch point 1: 0x888888

```

每次 watch_point 中的值更改后都应该再次 printf_wp

监视点：实际上这个是一个“能改大小”的静态链表，不需要 malloc，熟悉了链表操作实际上不难（我花时间手写了一下动态链表再看这儿结构体数组一开始还有点诧异）

另一个关键是要理解 static 的奥义所在，实际上都是为了将这个链表隔离在该作用范围内不要被其他文件访问，所以所有的操作都要在 sdb 和 watchpoint 里进行，不要对外暴露变量，只需要暴露函数能够使得他被调用即可。这也是程序隔离的重要性所在（一开始我觉得麻烦，但减小耦合的角度来说这非常重要，要做到职责分离），所有的数据结构访问和操作都在链表的 C 文件中执行，也不要对外暴露；而 CPU 每次执行完后的检查也是通过调用链表 C 的头文件进行函数执行（直接执行），做到职责分离和隔离。至于包一个宏，这个通过 RTFC 多多尝试即可得到结果。

### 实现函数

```c
WP* new_wp(char *exp); // new一个监视点
int free_wp(WP *wp);   // 成功返回1 失败返回0
bool check_wp();       // 判断监视点值是否改变
void display_wp();     // 打印监视点
bool delete_wp(int id);// 删除NO.为id的监视点
```

上面函数都是基于链表的操作，插入（头插还是尾插），删除（释放内存），遍历列表（扫描寻找）等操作比较基础。

注意如果监视点的值发生了改变，需要打印出来它的详细信息，即编号、表达式、旧值、新值，然后一定要把旧值更新成新的值！

### 是否激活监视点

> 由于监视点的功能需要在cpu_exec()的每次循环中都进行检查, 这会对NEMU的性能带来较为明显的开销. 我们可以把监视点的检查放在trace_and_difftest()中, 并用一个新的宏 CONFIG_WATCHPOINT把检查监视点的代码包起来; 然后在nemu/Kconfig中为监视点添加一个开关选项, 最后通过menuconfig打开这个选项, 从而激活监视点的功能. 当你不需要使用监视点时, 可以在menuconfig中关闭这个开关选项来提高NEMU的性能.

不太熟悉 `make menuconfig` “玩了玩”大概理解就是通过一些配置可以自动生成定义宏的文件，因此可以通过`IFDEF()`来选择执行某些函数功能。

具体的发现了最初`NEMU Configuration Menu/Testing and Debugging`中有个选项`Enable differential testing` 于是在Kconfig文件在找了下对应代码

```c
config DIFFTEST
  depends on TARGET_NATIVE_ELF
  bool "Enable differential testing"
  default n
  help
    Enable differential testing with a reference design.
    Note that this will significantly reduce the performance of NEMU.
```

感觉这个和`watchpoint`很像，都会花费一定代价`debug`于是就效仿一下写了下面的代码

```c
config WATCHPOINT
  depends on TARGET_NATIVE_ELF
  bool "Enable watchpoint"
  default n
  help
    Enable watchpoint with a reference design.
    Note that this will significantly reduce the performance of NEMU.
```

最后只需把之前代码改 `IFDEF(CONFIG_WATCHPOINT, packet_check_wp());` 就可以通过 `make menuconfig` 选择激活监视点的功能。
#### 配置系统 kconfig

在一个有一定规模的项目中, 可配置选项的数量可能会非常多, 而且配置选项之间可能会存在关联, 比如打开配置选项A之后, 配置选项B就必须是某个值. 直接让开发者去管理这些配置选项是很容易出错的, 比如修改选项A之后, 可能会忘记修改和选项A有关联的选项B. 配置系统的出现则是为了解决这个问题.

NEMU中的配置系统位于`nemu/tools/kconfig`, 它来源于GNU/Linux项目中的kconfig, 我们进行了少量简化. kconfig定义了一套简单的语言, 开发者可以使用这套语言来编写"配置描述文件". 在"配置描述文件"中, 开发者可以描述:

- 配置选项的属性, 包括类型, 默认值等
- 不同配置选项之间的关系
- 配置选项的层次关系