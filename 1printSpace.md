题目中通常要求输出的 n 个数字，两个数字之间用一个空格分隔，开头结尾没有空格。如果直接 `printf("%d ", n)` 的话，则会在末尾多出一个空格，本文讲述几种不留空格的方式和一个误区。

# 不留空格的方式

1. 方法一： 数列长度不确定的情况。设置标记，输出第一个数的时候后边不带空格，输出 2～n 个数的时候在数前边带空格。如下面代码中的 firstPrint。
2. 方法二： 已知数列长度的情况。当输出第 1~n-1 个数的时候在数后边带空格，输出第 n 个数的时候在数后边不带空格。因此需要用下标判断i是不是头/尾的数。当然已知数列长度的情况也可以采取方法一

```c
#include <stdio.h>
int main() {
    double balance[5] = {1000.56, 2.9, 3.4, 7.2, 59.8};
    int len = 4;
    // 末尾会多一个空格
    for (int i = 0; i <= len; i++) {
        printf("%g ", balance[i]);
    }
    printf("<- space there?\n");

    // 末尾不会多一个空格的方法 1
    int firstPrint = 0;
    for (int i = 0; i <= len; i++) {
        if (firstPrint == 0) {
            printf("%g", balance[i]);
            firstPrint = 1;
        } else {
            printf(" %g", balance[i]);
        }
    }
    printf("<- space there?\n");

    // 末尾不会多一个空格的方法 2
    for (int i = 0; i <= len; i++) {
        if (i == len) {
            printf("%g", balance[i]);
        } else {
            printf("%g ", balance[i]);
        }
    }
    printf("<- space there?\n");
    return 0;
}
``` 

# 误区

`\b` 可以删掉多余的一个空格，实际上不是的，只是移动了光标位置，输出的内容并没有被删除，见下面代码

[C语言中的转义字符\b的含义 码农哈里 2019-06-03](https://blog.csdn.net/harryduanchina/article/details/90751355)
> 总结：\b的含义是，将光标从当前位置向前（左）移动一个字符（遇到\n或\r则停止移动），并从此位置开始输出后面的字符（空字符\0和换行符\n除外）。

```c
#include <stdio.h>
int main() {
    double balance[5] = {1000.56, 2.9, 3.4, 7.2, 59.8};
    int len = 4;
    printf("请注意观察下面，看上去末尾没有多一个空格\n");
    for (int i = 0; i <= len; i++) {
        printf("%g ", balance[i]);
    }
    printf("\b"); // C语言中的转义字符\b
    printf("<- space there?\n");

    printf("请注意观察下面5是否被删除了?\n");
    printf("12345");
    printf("\b");

    return 0;
}
``` 
