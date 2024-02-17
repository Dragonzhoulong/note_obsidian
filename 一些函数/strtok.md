`strtok` 是 C 语言中的一个字符串处理函数，用于将一个字符串拆分成多个子字符串。它在 `string.h` 头文件中声明，并以以下形式定义：
```c
char *strtok(char *str, const char *delimiters);

```

- `str` : 要拆分的字符串。在第一次调用时，传入要拆分的字符串；在后续调用时，传入 NULL 以继续从上次位置继续拆分。
- `delimiters`: 用于指定拆分子字符串的分隔符字符串。

`strtok`会将原始字符串中的分隔符替换为`NULL`，并返回每个子字符串的指针。当没有更多的子字符串可供返回时，函数返回`NULL`。

请注意，`strtok`是一个在多线程环境下不安全的函数，如果您需要在多线程中使用字符串拆分，请考虑使用线程安全的替代函数。

下面是一个简单的示例，演示如何使用 `strtok` 函数将一串逗号分隔的数字拆分成多个子字符串：
```c
#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "1,2,3,4,5";
    const char delimiters[] = ",";

    char *token = strtok(str, delimiters); // 第一次调用strtok
    while (token != NULL) {
        printf("%s\n", token);
        token = strtok(NULL, delimiters); // 后续调用strtok，传入NULL以继续拆分
    }

    return 0;
}

```

输出结果
```
1
2
3
4
5
```

