Nvboard 如何配置的

语言: Verilog+cpp 
工具: verilator, makefile, nvboard
文件: 
	$(TOPNAME).nxdc
	$(TOPNAME).cpp
	$(TOPNAME).v
利用 makefile 来构建项目
```
需要更改 $(TOPNAME)

创建$(TOPNAME).cpp需要引入头文件 `#include<VTOPNAME.v>`
`#include<nvbooard>`

```

利用 python 脚本文件将 `.nxdc` 文件转化成 ` auto_bind_pins. cpp ` 来配置引脚

总而言之每次创建新项目的

- 建立组合逻辑电路模型使用阻塞赋值，建立时序逻辑电路模型使用非阻塞赋值。
- 用 always 块建立组合逻辑电路模型使用阻塞赋值。同一个 always 块中同时建立时序和组合逻辑电路模型使用非阻塞赋值。在同一个 always 块中不要既用非阻塞赋值又用阻塞赋值，且不要在一个以上的 always 块中为同一个变量赋值。
- 计数器 count++是阻塞赋值自加，等同于 count=count+1。
- 因为 Verilog 中的 for 循环没有类似 C 语言的 break 跳出机制，所以如果要让程序始终取的是左侧最高位的值，只需让 for 循环以正序从低位不断遍历至高位即可。

