##  readline 基本介绍
`readline` 是一个用于在 C 语言中实现命令行行编辑的库。它提供了一组功能强大的函数，可以用于读取用户输入，并支持编辑、自动补全、历史记录等功能，使得命令行界面更加友好和易于使用。

1. **基本功能：** `readline` 库的主要功能是读取用户在命令行中输入的文本。通过调用 `readline()` 函数，程序可以等待用户输入，并在用户按下回车键后获取输入的文本。例如：
```c
#include <stdio.h>
#include <readline/readline.h>

int main() {
    char* input = readline("Enter your name: ");
    printf("Hello, %s!\n", input);
    free(input);
    return 0;
}

```

2. **行编辑功能：** `readline` 库提供了丰富的行编辑功能，使得用户可以方便地编辑之前输入的文本。用户可以使用光标键、删除键、方向键等来修改输入的文本。这些行编辑功能让命令行操作更加流畅和直观。
    
3. **历史记录：** `readline`库可以自动保存用户输入的历史记录，并允许用户通过按上下方向键来浏览和重用之前输入过的命令或文本。这对于在命令行中反复执行某些命令或查找历史输入非常有用。
    
4. **自动补全：** `readline`库支持基本的自动补全功能，可以根据用户输入的部分文本来推断可能的命令或选项，并提供候选列表供用户选择。用户可以按下Tab键来触发自动补全功能。
    
5. **配置和定制：** `readline`库允许用户对其行为进行配置和定制。您可以修改提示符、调整历史记录的保存行数、启用或禁用自动补全等功能。
    
6. **可扩展性：** `readline` 库允许程序员通过编写回调函数来扩展其功能。这使得开发人员可以根据自己的需求定制和增强命令行交互体验。

>为了使用 `readline` 库，您需要链接到 `libreadline` 和 `libhistory` 库。在编译时，通常需要添加 `-lreadline -lhistory` 选项。
>
请注意，`readline` 库不是 C 语言标准库的一部分，因此在使用它之前，您可能需要在系统中安装相应的库文件。在 Linux 系统上，可以使用包管理工具来安装 `readline` 库。


### 历史记录功能


`   add_history()` 函数的功能是将用户输入的文本添加到历史记录中。当用户在命令行中按下 Enter 键提交输入时，通常会调用此函数，将输入的命令添加到历史记录中。

一旦添加到历史记录中，用户可以使用上下箭头键来遍历历史记录，并重新执行或修改之前输入的命令。
```c
#include <stdio.h>
#include <readline/readline.h>
#include <readline/history.h>

int main() {
    char* input;

    // 使用readline读取用户输入并添加到历史记录中
    while ((input = readline("Enter a command (or 'exit' to quit): ")) != NULL) {
        if (strcmp(input, "exit") == 0) {
            free(input);
            break;
        }

        printf("You entered: %s\n", input);

        // 将用户输入添加到历史记录中
        add_history(input);

        free(input);
    }

    return 0;
}

```
在这个示例中，我们使用`readline()`函数读取用户输入，并将输入的文本添加到历史记录中，以便用户可以使用上下箭头键来遍历和查看之前输入的命令。注意在程序结束前要使用`free()`函数释放通过`readline()`获得的动态分配的内存。

## readline 自动补全功能
自动补全的实现通常涉及使用 `readline()` 库提供的特定函数来配置自动补全行为。在下面的示例中，我们将使用 `readline()` 和相关函数来实现一个简单的自动补全功能，其中将自动补全用户输入的颜色名称。

请确保您的系统已经安装了`readline`库，否则需要先进行安装。
```c
#include <stdio.h>
#include <stdlib.h>
#include <readline/readline.h>
#include <readline/history.h>

// 定义一些颜色名称
char* colors[] = {"red", "green", "blue", "yellow", "orange"};

// 自定义自动补全函数
char** color_completion(const char* text, int start, int end) {
    rl_attempted_completion_over = 1; // 告知readline不再尝试其他默认的自动补全方式

    // 查找与text部分匹配的颜色名称
    char** matches = NULL;
    int i, count = 0;
    for (i = 0; i < sizeof(colors) / sizeof(colors[0]); i++) {
        if (strncmp(colors[i], text, end) == 0) {
            matches = (char**)realloc(matches, (count + 2) * sizeof(char*));
            matches[count++] = strdup(colors[i]);
        }
    }
    matches[count] = NULL;

    return matches;
}

int main() {
    // 配置自动补全函数
    rl_attempted_completion_function = color_completion;

    char* input;
    while ((input = readline("Enter a color (or 'exit' to quit): ")) != NULL) {
        if (strcmp(input, "exit") == 0) {
            free(input);
            break;
        }

        printf("You entered: %s\n", input);
        add_history(input);
        free(input);
    }

    return 0;
}

```



在这个示例中，我们定义了一个颜色名称数组`colors[]`，其中包含一些颜色名称。然后，我们自定义了`color_completion()`函数来实现自动补全功能。这个函数在用户输入文本时，会找到与文本部分匹配的颜色名称，并返回一个字符串数组，其中包含所有匹配的颜色名称。

在`main()`函数中，我们通过`rl_attempted_completion_function`变量将自定义的自动补全函数`color_completion()`注册为`readline()`库的自动补全函数。然后，我们使用`readline()`函数来读取用户输入，并在用户输入颜色名称后，打印出输入的文本。用户还可以输入"exit"来退出程序。

请注意，这只是一个简单的示例来演示如何使用 `readline()` 库实现自动补全功能。在实际项目中，可能需要更复杂的自动补全逻辑和更全面的候选项列表。自动补全的实现还可以结合其他功能，例如命令历史记录和特定环境下的自定义补全逻辑。

