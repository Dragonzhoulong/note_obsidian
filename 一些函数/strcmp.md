### Strcmp


### Strncmp
`strncmp` 是 C 语言中的字符串部分比较函数，用于比较两个字符串的前若干个字符是否相等。它在 `string.h` 头文件中声明，并以以下形式定义：
```c
int strncmp(const char *str1, const char *str2, size_t n);

```
- `str1` 和 `str2` ：要进行比较的两个字符串。
- `n`：指定要比较的字符数。

`strncmp`返回一个整数值，具有以下含义：

- 如果`str1`的前`n`个字符与`str2`的前`n`个字符相等，则返回0。
- 如果`str1`在前`n`个字符上在字典序中排在`str2`之前，则返回一个负整数。
- 如果`str1`在前`n`个字符上在字典序中排在`str2`之后，则返回一个正整数。

与`strcmp`不同的是，`strncmp`可以指定要比较的字符数，因此可以用于比较两个较长字符串的前若干个字符。

下面是一个简单的示例，演示如何使用 `strncmp` 函数来比较两个字符串的前几个字符：
```c
#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "hello world";
    char str2[] = "hello everyone";

    int result1 = strncmp(str1, str2, 5); // 比较前5个字符

    if (result1 == 0) {
        printf("The first 5 characters of str1 and str2 are equal.\n");
    } else if (result1 < 0) {
        printf("str1 comes before str2 in the first 5 characters.\n");
    } else {
        printf("str1 comes after str2 in the first 5 characters.\n");
    }

    int result2 = strcmp(str1, str2); // 完全比较

    if (result2 == 0) {
        printf("str1 and str2 are equal.\n");
    } else if (result2 < 0) {
        printf("str1 comes before str2.\n");
    } else {
        printf("str1 comes after str2.\n");
    }

    return 0;
}

```

