
结论： 为了安全考虑，用 fgets(b,sizeof(b),stdin) 替代 gets(b)

下面是 gets.c 的代码

```c
#include <stdio.h>
int main() {
    char b[40];
    gets(b);
    printf("%s", b);
    return 0;
}
``` 

使用 gcc 10.2.1 进行编译（`gcc gets.c -o gets`）会提示 `warning: implicit declaration of function 'gets' is invalid in C99 [-Wimplicit-function-declaration]` 和 `warning: the `gets' function is dangerous and should not be used.`，当然只是警告，程序还是可以正常运行的。 

强烈建议使用 fgets 替代 gets，使用案例如下：

```c
#include <stdio.h>
int main() {
    char b[40];
    fgets(b,sizeof(b),stdin);
    printf("%s", b);
    return 0;
}
```

# 异常情况演示

```c
#include <stdio.h>

int main() {
    char a;
    char name[3];
    char b;
    printf("input char a:");
    a = getchar();
    printf("value of char a:%c\n", a); //第一次输出a的值
    // 清除输入缓冲区中的换行符，不然就可能把回车赋值给name了
    while (getchar() != '\n') {
        // 空循环体，只是为了消耗输入缓冲区中的字符
    }
    printf("input chars name:");
    gets(name);
    printf("value of chars name:%s\n", name);
    printf("input char b:");
    b = getchar();
    //输出所有的值，注意a的值不对劲了
    printf("a:%c; name:%s; b:%c \n", a, name, b);
}
```

运行结果如下：

    input char a:a
    value of char a:a
    input chars name:1234567890
    value of chars name:1234567890
    input char b:b
    a:4; name:1234567890; b:b 

可以看到变量 a 的值刚开始输出是对的，但是最后再次输出时，就变化了，然而代码没有修改 a。原因是输入 name 是用 gets，同时输入长度超出了限制，代码编译后不管这个情况，照样把这个值往内存里填，从而影响了其它变量的值

下面是用 fgets 的例子，同时在输入 b 之前也进行了清空输入缓冲区域，可以注意到这回超出长度限制的 name 输入没有影响其它变量的值

```c
#include <stdio.h>

int main() {
    char a;
    char name[3];
    char b;
    printf("input char a:");
    a = getchar();
    printf("value of char a:%c\n", a);
    while (getchar() != '\n') {
    }
    printf("input chars name:");
    fgets(name, sizeof(name), stdin); // 使用 fgets 代替 gets
    printf("value of chars name:%s\n", name);
    while (getchar() != '\n') {
    }
    printf("input char b:");
    b = getchar();
    // 输出所有的值，注意a的值没有发生变化
    printf("a:%c; name:%s; b:%c \n", a, name, b);
}
```

# Refer

[关于gets()的危险（不安全）性 C 2018/04/08](https://otterv.github.io/2018/04/08/201804gets/)
