*2021/4/29*

# [C++]利用位运算实现的子集算法

## 对应关系

假设有一个集合`{a, b, c, d, e, f, g, h}`拥有八个元素，我们选择利用一个长度为一个字节的变量`flag`来存储其状态。

当`flag`等于0000_0000时，不输出元素；

当`flag`等于0000_0001时，输出位数为1对应的集合元素`h`;

当`flag`等于0010_1111时，输出`c, e, f, g, h`

`flag`继续自增，直到等于1111_1111时执行最后一次运算。

集合子集数量为2n；

n位二进制数对应状态也恰好为2n，易得集合与二进制状态存在一一对应关系。

由于64位电脑只能支持最大64位数据，也就是集合的最大长度位64，若想增加其上限，使用`bitset`类。

`bitset`并没有实现加法，我们可以自己实现。

## 位运算加法

```
加法运算
0 + 0 = 0
0 + 1 = 1
1 + 0 = 1
1 + 1 = 0

异或运算
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0

显然加法运算与异或运算存在某种对应关系，但这没有解决问题，因为还有进位，接下来看一下与运算。

与运算
0 & 0 = 0 // 不进位 -> 0 = 0 << 0
0 & 1 = 0 // 不进位 -> 0 = 0 << 0
1 & 0 = 0 // 不进位 -> 0 = 0 << 0
1 & 1 = 1 // 进位 -> 10 = 1 << 1

显然，位运算中进位操作就是将其&运算结果左移1位
结合以上推断，公式为：a + b = ((a & b) << 1) + (a ^ b)，但加法是无法使用的，因此我们还需要将其拆分。

接下来看一下两位位运算
普通运算
11 + 10 = 101

位操作运算
第一次、
11 ^ 10 = 01
(11 & 10) << 1 = 100

根据第一次的结果计算第二次、
01 ^ 100 = 101
(01 & 10) << 1 = 0


可见，我们只需要循环直到&运算结果中没有1，^运算即为结果。
```

完整代码如下

```c++
#include <iostream>
#include <bitset>
#include <climits>
#include <cstring>

#define N 8

using namespace std;

int main() {
    int n;
    scanf("%d", &n);

    char e[] = {'a', 'b', 'c', 'd', 'e'};

    /* 空间开销较大，选择在堆上分配内存 */

    bitset<N> *flag = new bitset<N>(); // 用于记录子集状态
    bitset<N> *one = new bitset<N>(1); // 0...01，用于相加
    bitset<N> *zero = new bitset<N>(); // 0...00，用于比较

    // 为a加1准备的变量
    bitset<N> *_and = new bitset<N>(*flag & *one);
    bitset<N> *_xor = new bitset<N>(*flag ^ *one);
    bitset<N> *t1 = new bitset<N>();
    bitset<N> *t2 = new bitset<N>();
    
    // 用于查询flag中为1的位置
    bitset<N> *cursor = new bitset<N>(1);

    bool isEnd = false;
    while (!isEnd) {
        if (flag->count() == n) {
            isEnd = true; // 执行完这轮循环
        }

        /* 检测为1的位置 */
        ////////////////////////////START
        for (int i = sizeof(e)-1; i >= 0; i--, *cursor <<= 1) {
            if ((*flag & *cursor) != *zero) {
                printf("%c", e[i]);
            }
        }
        *cursor = 1;
        ////////////////////////////END

        printf("\n");


        /* a加1 */
        ////////////////////////////START
        while (_and->any()) {
            *t1 = *_xor;
            *t2 = *(_and) << 1;
            *_and = *t1 & *t2;
            *_xor = *t1 ^ *t2;
        }
        *flag = *_xor;  
        ////////////////////////////END
        
        *_and = *flag & *one;
        *_xor = *flag ^ *one;
    }

    delete flag, one, zero, _and, _xor, t1, t2, cursor;

    return 0;
}
```

运算结果

![此图片的alt属性为空；文件名为image-1.png]([C++]利用位运算实现的子集算法.assets/image-1.png)