# tips
`gcc -E a.c | vim -` 将输出重定向给 vim 打开

`-` means that vim reads form the standard input instead of a file name;

	预处理器就是将 `#include ` 的东西复制粘贴进去并且去除注释
## 头文件是如何找到的?
`gcc -E a.c –verbose > /dev/null`
```
#include "..." search starts here:
#include <...> search starts here:
 /usr/lib/gcc/x86_64-linux-gnu/9/include
 /usr/local/include
 /usr/include/x86_64-linux-gnu
 /usr/include
End of search list.
```

`man gcc` 搜索 -I 可得知头文件搜索的顺序

```
    The lookup order is as follows:

           1.  For the quote form of the include directive, the directory of the current file is searched first.

           2.  For the quote form of the include directive, the directories specified by -iquote options are searched in left-to-right order, as they appear on the command line.

           3.  Directories specified with -I options are scanned in left-to-right order.

           4.  Directories specified with -isystem options are scanned in left-to-right order.

           5.  Standard system directories are scanned.

           6.  Directories specified with -idirafter options are scanned in left-to-right order.
```

## 验证

`gcc -E a.c –Iaaa -Ibbb -verbos > /dev/null

有了-Iaaa 后就会先去 aaa dir 去找 head file


### 类函数宏
`#define max(a,b) ((a)>(b)?(a):(b))`
预处理阶段仅仅进行文本粘贴, 不求值
+ 小心优先级
	+ 所以需要用括号包裹参数
+ 小心副作用
	+ 好的编程习惯 -> 一个参数不要展开多次
	
```
	max(i++,j++)
	=>
   ((i++)>(j++)?(i++):(j++));
   多次i++,j++参数产生了变化
```

+ 怎样避免参数多次展开呢?
```
#define max(a,b) ({int x =a;int y=b;x>y?x:y})
```

使用 temp 变量来进行

### 预处理的其他工作
+ 去掉注释
+ 链接因为断行符 (\) 而拆分的字符串
+ 处理条件编译 `#ifdef` `#else ` `#endif` 

`riscv64-linux-gnu-gcc -E a.c `
有如下输出
```
# 5 "a.c"
int main() {
  printf("Hello World!\n" );

  printf("Hello RISC-V!\n");

  ((1)>(2)?(1):(2));
  ((i++)>(j++)?(i++):(j++));
  return 0;
}
```

+ 字符串化 `#`
+ 标识符链接 `##`
```
#define _str(x) #x
#define _concat(a,b) a##b
_concat(pr,intf)(_str(RISC-V));
```

更多的宏...

+ predefined macro
`_FILE_`, `__DATE__`,

## 编译
---
词法分析 -> 语法分析 ->语义分析 -> 中间代码生成 -> 优化 -> 目标代码

### 词法分析
识别并记录源文件中的每一个 token
+ 标识符, 关键字, 常数, 字符串, 运算符, 大括号, 分号 (semicolon)
+ 还记录了 token 的位置 (文件名: 行号: 列号)
+ C 代码=文本: 本质上是一个文本匹配程序
+ 
`GCC -E a.c 2>&1 | wc` //2>&1
---------------
####  ccache

之前在虚拟机里已经搞好了, 但是好像没有记录下来蚌埠,

-------------


### make 相关的东西

make 