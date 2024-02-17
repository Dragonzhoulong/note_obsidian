整合出适合的 jmakefile

target:
	1. sim
	2. wave
	3. clean
``` shell
#生成的可执行文件名称  
TOPNAME = top  
#在指定文件夹中按扩展名查找所有源文件  
VSRC = $(shell find $(abspath ./vsrc) -name "*.v")  
CSRC = $(shell find $(abspath ./csrc) -name "*.c" -or -name "*.cc" -or -name "*.cpp")  
#nvbroad自动绑定源文件，注意，如果没有$(BUILD_DIR)目录，将无法正确的生成该文件  
SRC_AUTO_BIND = $(abspath $(BUILD_DIR)/auto_bind.cpp)  
CSRC += $(SRC_AUTO_BIND)  
#nvbroad约束文件  
CONSTR = constr/top.nxdc  
  
#构建目录、verilator目标目录、可执行文件存放路径  
BUILD_DIR = ./build  
OBJ_DIR = $(BUILD_DIR)/obj_dir  
BIN = $(BUILD_DIR)/$(TOPNAME)  
  
#传递给verilator的参数：以c++形式输出调试信息，并自动依据makefile进行构建  
VERIARG = -Wall --cc -MMD --build   
  
#传递给preproject连接器的信息，添加SDL2库信息  
LDFLAGS += -lSDL2 -lSDL2_image  
  
#包含文件路径，加前缀-I传递给g++，INC_PATH在nvbroad.mk中定义  
INCFLAGS = $(addprefix -I, $(INC_PATH))  
  
#传递给g++的编译参数，包括包含路径和TOP_NAME的定义  
CFLAGS += $(INCFLAGS)  -DTOP_NAME="\"V$(TOPNAME)\""  
  
#包含nvbroad.mk  
include $(NVBOARD_HOME)/scripts/nvboard.mk  
#新建构建目录，防止出现无法新建autobind.cpp的错误  
$(shell mkdir -p $(BUILD_DIR))  
  
#默认的构建对象  
sim:  $(CSRC) $(VSRC) $(NVBOARD_ARCHIVE)  
#提交当前存储区，提交信息为sim RTL  
	$(call git_commit, "sim RTL") # DO NOT REMOVE THIS LINE!!!  
	  
#运行verilator，传递给其编译参数  
#指定top模块名称  
#添加所有源文件和nvbroad库  
#传递所有preproject连接器参数，加前缀  
#传递所有编译器参数，加前缀  
#指定生成对象目录，生成可执行文件，输出路径为预先指定的可执行文件存储路径  
	verilator $(VERIARG) \  
		-top $(TOPNAME) \  
		$^ \  
		$(addprefix -LDFLAGS , $(LDFLAGS)) \  
		$(addprefix -CFLAGS , $(CFLAGS)) \  
		--Mdir $(OBJ_DIR)  --exe -o $(abspath $(BIN))  
  
#自动绑定文件，由于其被可执行文件依赖，会先生成该文件。调用python执行nvbroad自带的.py文件，输入约束文件，输出自动绑定文件  
$(SRC_AUTO_BIND): $(CONSTR)   
	python3 $(NVBOARD_HOME)/scripts/auto_pin_bind.py  $^ $@
```


## sim_main. cpp
>用来测试电路
``` c++
#include "verilated.h"
#include "verilated_vcd_c.h"
#include "Vtop.h"

VerilatedContext* contextp = NULL;
VerilatedVcdC* tfp = NULL;

static Vmux21* top;

void step_and_dump_wave(){
  top->eval();
  contextp->timeInc(1);
  tfp->dump(contextp->time());
}
void sim_init(){
  contextp = new VerilatedContext;
  tfp = new VerilatedVcdC;
  top = new Vmux21;
  contextp->traceEverOn(true);
  top->trace(tfp, 0);
  tfp->open("dump.vcd");
}

void sim_exit(){
  step_and_dump_wave();
  tfp->close();
}

int main() {
  sim_init();

  top->s=0; top->a=0; top->b=0;  step_and_dump_wave();   // 将s，a和b均初始化为“0”
                      top->b=1;  step_and_dump_wave();   // 将b改为“1”，s和a的值不变，继续保持“0”，
            top->a=1; top->b=0;  step_and_dump_wave();   // 将a，b分别改为“1”和“0”，s的值不变，
                      top->b=1;  step_and_dump_wave();   // 将b改为“1”，s和a的值不变，维持10个时间单位
  top->s=1; top->a=0; top->b=0;  step_and_dump_wave();   // 将s，a，b分别变为“1,0,0”，维持10个时间单位
                      top->b=1;  step_and_dump_wave();
            top->a=1; top->b=0;  step_and_dump_wave();
                      top->b=1;  step_and_dump_wave();

  sim_exit();
}

```



## autobind. cpp
>用来配置引脚'

## vscode
### Bing
如果您想在 C 中引入 `#include<verilated.h>`，您需要先下载并安装 verilator，然后在您的 `c_cpp_properties. json` 文件中添加 verilator 的头文件路径，例如：
```

[json]
"includePath": [
    "${workspaceFolder}/**",
    "/usr/local/share/verilator/include"
]
其中，/usr/local/share/verilator/include 是 verilator 的默认安装路径，您可能需要根据您的实际情况进行修改。
然后，在您的 tasks. json 文件中添加 verilator 的编译命令和参数，例如：

[json]
"command": "verilator",
"args": [
    "-Wall",
    "-cc",
    "top. v"
]
```
其中，top. v 是您的 Verilog 源文件，您可能需要根据您的实际情况进行修改。
最后，在您的 C 代码中引入<verilated. h>头文件，并使用 Verilated 类和相关方法来创建和操作 Verilog 模型。
以上就是引入`<verilated. h>`的方法。如果您对此还有疑问，请随时提出。😊


`gcc -E a.c | vim -` 将输出重定向给 vim 打开

`-` means that vim reads form the standard input instead of a file name;

`gcc -E 2>&1 | vim -`

