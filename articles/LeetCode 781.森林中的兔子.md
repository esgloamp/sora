*2021/4/4*

# LeetCode 781.森林中的兔子

将兔子分为两部分，一部分是回答了问题的兔子，令其为A组，另一部分是没有回答问题的兔子，令其为B组。

由题意可知，颜色相同的兔子回答的数字一样。

```c
int compare(const void *a, const void* b) {
    return *(int*)a - *(int*)b;
}

int numRabbits(int* answers, int answersSize){

    int outsize = 0; // B组数量
    qsort(answers, answersSize, sizeof(int), (void *)compare); 

    for (int i = 0; i < answersSize; i++) {
        int t = answers[i];
        if (t == 0) continue;
        int count = 0;
        for (int j = i+1; j <= i+t && j < answersSize; j++) {
            if (answers[j] == t) { // 找到了同颜色的兔子
                count++;
                answers[j]=0;
            }
        }
        answers[i] -= count;// A组中减去找到的同颜色兔子，剩余的就是该颜色在B组中的兔子数量
        outsize += answers[i];
    }
    return outsize+answersSize;
}
```

时间复杂度为 O(n2)

