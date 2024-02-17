Soc: System-on-a-Chip
SoPC: system  on a programmable Chip
嵌入式处理器内核
怎么自定义添加IP核
# PA 

## Vim’s interface is a programming language
	The most important idea in Vim is that Vim’s interface itself is a programming language. Keystrokes (with mnemonic names) are commands, and these commands _compose_. This enables efficient movement and edits, especially once the commands become muscle memory.
### Movement

You should spend most of your time in Normal mode, using movement commands to navigate the buffer. Movements in Vim are also called “nouns”, because they refer to chunks of text.

-   Basic movement: `hjkl` (left, down, up, right)
-   Words: `w` (next word), `b` (beginning of word), `e` (end of word)
-   Lines: `0` (beginning of line), `^` (first non-blank character), `$` (end of line)
-   Screen: `H` (top of screen), `M` (middle of screen), `L` (bottom of screen)
-   Scroll: `Ctrl-u` (up), `Ctrl-d` (down)
-   File: `gg` (beginning of file), `G` (end of file)
-   Line numbers: `:{number}<CR>` or `{number}G` (line {number})
-   Misc: `%` (corresponding item)
-   Find: `f{character}`, `t{character}`, `F{character}`, `T{character}`
    -   find/to forward/backward {character} on the current line
    -   `,` / `;` for navigating matches
-   Search: `/{regex}`, `n` / `N` for navigating matches

### Selection

Visual modes:

-   Visual: `v`
-   Visual Line: `V`
-   Visual Block: `Ctrl-v`

Can use movement keys to make selection.

### Edits

Everything that you used to do with the mouse, you now do with the keyboard using editing commands that compose with movement commands. Here’s where Vim’s interface starts to look like a programming language. Vim’s editing commands are also called “verbs”, because verbs act on nouns.

-   `i` enter Insert mode
    -   but for manipulating/deleting text, want to use something more than backspace
-   `o` / `O` insert line below / above
-   `d{motion}` delete {motion}
    -   e.g. `dw` is delete word, `d$` is delete to end of line, `d0` is delete to beginning of line
-   `c{motion}` change {motion}
    -   e.g. `cw` is change word
    -   like `d{motion}` followed by `i`
-   `x` delete character (equal do `dl`)
-   `s` substitute character (equal to `cl`)
-   Visual mode + manipulation
    -   select text, `d` to delete it or `c` to change it
-   `u` to undo, `<C-r>` to redo
-   `y` to copy / “yank” (some other commands like `d` also copy)
-   `p` to paste
-   Lots more to learn: e.g. `~` flips the case of a character

### Counts

You can combine nouns and verbs with a count, which will perform a given action a number of times.

-   `3w` move 3 words forward
-   `5j` move 5 lines down
-   `7dw` delete 7 words

### Modifiers

You can use modifiers to change the meaning of a noun. Some modifiers are `i`, which means “inner” or “inside”, and `a`, which means “around”.

-   `ci(` change the contents inside the current pair of parentheses
-   `ci[` change the contents inside the current pair of square brackets
-   `da'` delete a single-quoted string, including the surrounding single quotes


## about Makefile
1. 跟我一起学makefile

## about docker
## How to read code and skip
## grep
G R E P 正则表达式=> **G**lobal **R**egular **E**xpression **P**rint 
```shell
grep -n main $(find . -name "*.c") # RTFM: -n
find . | xargs grep --color -nse '\<main\>'
```
第一条命令会找出来所有有main的地方
但是比如说mianXXXadw 也会被搜出来
第二条命令说明了单词的边界
********

### Strtok
再就是一系列的 string 系列

[](PA/strtok.md)

直接运行./build/riscv64-nemu-interpreter 出现重复输出


#### 表达式regex
正则表达式在 C 语言中如何实现?

在C语言中，可以使用`regex.h`头文件中的函数来实现正则表达式的功能。以下是实现正则表达式的基本步骤：

1. 引入头文件：在C程序中，需要包含`regex.h`头文件以访问正则表达式相关功能。
```c
#include <regex.h>
```

2. 编译正则表达式：使用`regcomp()`函数将正则表达式编译为一个模式对象。该函数接受三个参数：模式对象指针、正则表达式字符串和编译选项。


``` c

regex_t regex; 
int ret = regcomp(&regex, "your_regex_pattern", 0); 
if (ret) {     
fprintf(stderr, "Failed to compile regex\n");     exit(1); 
}
```

3. 匹配字符串：使用`regexec()`函数匹配正则表达式和输入字符串。该函数接受五个参数：模式对象、输入字符串、匹配的最大数目、匹配位置的数组和匹配标志。
```c

int match_result = regexec(&regex, "your_input_string", 0, NULL, 0); 
if (!match_result) {
printf("String matches regex\n"); 
} 
else if (match_result == REG_NOMATCH) 
{     
	printf("String does not match regex\n"); 
} 
else {     
	char error_message[100];     
	regerror(match_result, &regex, error_message, sizeof(error_message));     
	fprintf(stderr, "Regex match failed: %s\n", error_message);     
	exit(1); }
```

4. 释放资源：使用`regfree()`函数释放之前编译的正则表达式模式对象。
	`regfree(&regex);`

需要注意的是，使用正则表达式的相关函数时，请确认编译器是否支持`regex.h`头文件。不同的编译器可能具有不同的实现和支持程度。

#### regex 的简单介绍
1. 元字符 (meta characters)

| 元字符 | 描述                                           |
| ------ | ---------------------------------------------- |
| .      | 句号匹配任意 `单个` 字符除了换行符             |
| []     | 字符种类. 匹配方括号内的任意字符               |
| \[^\]  | 否定的字符种类                                 |
| *      | 匹配>=0 个重复在\*号前的字符                   |
| +      | 匹配>=1 个重复在+号前的字符                    |
| ?      | 标记? 之前的字符是可选的                       |
| {n, m} | 匹配 num 个大括号之前的字符或字符集{n<=num<=m} |
| (xyz)  | 匹配和 xyz 完全相等的字符串                    |
| \|     | 或运算符, 匹配符号前或后的字符                 |
| \      | 转义字符, 用于匹配一些保留的字符例如 `[] () {}`           | 
2. 示例
2.2 字符集
---
```markdown
"[Tt]he" => `The` car parked in `the` garage
```


### 我想用 regex 来作什么?
提取出 number, 一些运算符

在 Ubuntu 平台下，正则表达式中需要使用两个反斜杠（\）来进行转义的原因是因为反斜杠在 Unix/Linux 系统中有特殊的含义。单个反斜杠被用作转义字符，用于指示后面的字符应该被解释为特殊字符而不是字面量。

例如，如果你想匹配一个句号（.），你需要在正则表达式中使用两个反斜杠来转义它，即"\."。第一个反斜杠用来转义第二个反斜杠，告诉正则表达式引擎将其视为字面量的句号。

这种行为与编程语言中的字符串转义类似，因为在许多编程语言中，反斜杠也被用作转义字符。因此，在正则表达式中使用两个反斜杠来转义特殊字符可以保持一致性，并与其他代码保持统一。

