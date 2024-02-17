Strtol (str to long number)

`strtol` 是一个 C 标准库函数，用于将字符串转换为长整型数（long integer）。该函数在 C 标准库 `<stdlib.h>` 中声明，并提供了将字符串表示的整数转换为长整型数的功能。

函数原型如下：
```c
long strtol(const char *str, char **endptr, int base);

```

参数说明：

- `str`：要进行转换的字符串指针。
- `endptr`：指向字符指针的指针，用于指示转换过程中停止的位置。如果为NULL，则不需要返回停止位置信息。
- `base`：表示要转换的进制。通常为 10（十进制），也可以是其他值（如 16 表示十六进制）。

返回值：

- 函数返回被转换的长整型数。

`strtol` 函数会从字符串的开头开始解析，跳过前导空格字符，然后根据指定的进制将后续的字符转换为长整型数，直到遇到非数字字符或字符串结束为止。如果遇到非数字字符或字符串为空，则转换结束。

函数同时还会处理正负号，即字符串中出现的正负号会影响结果的正负性。如果字符串开头是正号，则结果为正；如果字符串开头是负号，则结果为负。

示例用法：
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    const char* str = "12345";
    char* endptr;
    int base = 10;

    long result = strtol(str, &endptr, base);

    if (endptr == str) {
        printf("No digits were found.\n");
    } else if (*endptr != '\0') {
        printf("Conversion stopped at: %c\n", *endptr);
    } else {
        printf("Converted value: %ld\n", result);
    }

    return 0;
}

```
输出结果：
```bash
Converted value: 12345
```

