结论
1. scanf 的变量要匹配对应的格式化字符串。float f, double lf, char c
2. 编译器提示的错误要消除，不消除不能运行；同时尽量消除警告

# double f

c语言中，给 double 类型的变量用 scanf %f 输入赋值时，会发生逻辑上的错误，请看代码

```c
#include <stdio.h>

int main() {
    double value;
    // 错误的用法
    printf("Enter a double value: ");
    scanf("%f", &value); // 这可能会导致问题
    printf("Incorrect usage: You entered: %lf\n", value);

    // 正确的用法
    printf("Enter a double value: ");
    scanf("%lf", &value);
    printf("Correct usage: You entered: %lf\n", value);
    return 0;
}
```

运行结果如下：

    Enter a double value: 1.234
    Incorrect usage: You entered: 0.000000
    Enter a double value: 1.234
    Correct usage: You entered: 1.234000

可以看到第一次用 f 来输入时，其并没有把 1.234 赋值给变量，输出变量的值是 0.000000。

其实编译器是有警告的： warning: format specifies type 'float *' but the argument has type 'double *' [-Wformat]

# float lf

c语言中，给 float 类型的变量用 scanf %lf 输入赋值时，会发生逻辑上的错误，请看代码

```c
#include <stdio.h>

int main() {
    float value;
    // 错误的用法
    printf("Enter a float value: ");
    scanf("%lf", &value); // 这可能会导致问题
    printf("Incorrect usage: You entered: %lf\n", value);

    // 正确的用法
    printf("Enter a float value: ");
    scanf("%f", &value);
    printf("Correct usage: You entered: %lf\n", value);
    return 0;
}
```

运行结果如下：

    Enter a float value: 1.234
    Incorrect usage: You entered: -369098.750000
    Enter a float value: 1.234
    Correct usage: You entered: 1.234000

可以看到第一次用 lf 来输入时，其并没有把 1.234 赋值给变量，输出变量的值是 -369098.750000。两者毫无关系

其实编译器是有警告的：format specifies type 'double *' but the argument has type 'float *' [-Wformat]

# char s

c语言中，给 char 类型的变量用 scanf %s 输入赋值时，会发生逻辑上的错误，请看代码

```c
#include <stdio.h>

int main() {
    char value;

    // 错误的用法
    printf("Enter a char value: ");
    scanf("%s", &value); // 这可能会导致问题
    printf("Incorrect usage: You entered: %c\n", value);
    // 正确的用法
    printf("Enter a char value: ");
    scanf("%c", &value);
    printf("Correct usage: You entered: %c\n", value);
    return 0;
}
```

运行结果如下：

    Enter a char value: q
    Incorrect usage: You entered: q
    Enter a char value: Correct usage: You entered: 
    


可以看到第一次用 s 来输入时，其把 q 赋值给变量，输出变量的值是 q。但是第二次我都没有输入，就运行结束了，这是为什么呢？则此编译器没有警告了，我们把代码顺序换一下运行就知道为什么了


```c
#include <stdio.h>

int main() {
    char value;
    // 正确的用法
    printf("Enter a char value: ");
    scanf("%c", &value);
    printf("Correct usage: You entered: %c\n", value);
    // 错误的用法
    printf("Enter a char value: ");
    scanf("%s", &value); // 这可能会导致问题
    printf("Incorrect usage: You entered: %c\n", value);
    return 0;
}
```

运行结果如下：

    Enter a char value: ab
    Correct usage: You entered: a
    Enter a char value: Incorrect usage: You entered: b

可以看到第一次输入 ab 两个字符，第一个字符 a 赋值给 value 了，第二次我没有输入，直接输出了 b,说明第一次只从输入中读取了一个字符。这个时候就可以说明上面最开始用 %s 输入为什么会出问题，上面输入 q 然后回车，q 确实赋值成功了，但第二次没有输入就结束了，因为它把 回车 识别为输入了，也就是说 用 %s 给 char 类型赋值会给后面的赋值留下回车隐患。
