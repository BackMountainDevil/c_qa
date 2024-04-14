结论：计算小数时优先选 double，而不是 float

《庄子·天下》
> 一尺之棰，日取其半，万世不竭。

一米的棍子，一天砍掉一半，问第 n 天(1~20)时被砍掉的总长度是多少？类似的有小球落地反弹一半的路程，下面的代码求的是小球从 50 米高空落地反弹的路程，结果保留十位小数，代码看起来没啥问题，当输入 20 的时候结果是 149.9998321533，而把 float 都换成 double 后结果则变成了 149.9998569489，两者在第五位小数就开始不同了，是算错了吗？不是的，用 python 算出来的结果(下面第二个代码)是 149.99985694885254，float 和 double 的计算结果又比较相近，是什么导致了这一个问题？

通俗的来说是两者的精度导致的问题，误差一点点的积累下来的。float 单精度，能保留 7 位小数；double 双精度，能保留 15 位小数。也就是说小球反弹的高度不足 0.0000001 时，float 计算就会出问题了。下面第三个代码演示了这个问题

```c
// 从 50 米高空落地反弹的路程
#include <stdio.h>
int main()
{
    float h = 50.0;
    float d = 2.0;
    float sum = 0;
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        sum = sum + h;
        h = h / d;  // 注意这里没有直接写 h=h/2
        sum = sum + h;
    }
    printf("%.10f", sum);

    return 0;
}
```


```python
# 计算 20 次落地的路程
h=50.0
s=0
for _ in range(20):
    s+=h
    h/=2
    s+=h
print(s)    # 149.99985694885254
```


下面的代码运行结果是：

    hg!=hm
    3.123457 3.123457
    3.123457 3.123457
    3.123456789012346 3.123456716537476

```c
#include <stdio.h>
int main()
{
    double hm = 3.1234567890123456;
    float hg = hm;
    if (hg == hm) {
        printf("hg==hm\n");
    } else {
        printf("hg!=hm\n");
    }
    printf("%lf %lf\n", hm, hg); // lf f 都默认输出 6 位小数，四舍五入
    printf("%f %f\n", hm, hg);
    printf("%.15lf %.15lf\n", hm, hg); // 超出精度就开始不对劲了
    return 0;
}
```

# Refer

- [Difference between float and double in C/C++. 23 Jun, 2023](https://www.geeksforgeeks.org/difference-float-double-c-cpp/)
