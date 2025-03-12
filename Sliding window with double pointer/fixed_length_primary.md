# 定长滑动窗口（基础题）

## [1456.定长字串中元音的最大数目]([1456. 定长子串中元音的最大数目 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/description/))

给你字符串s和整数k。

请返回字符串s中长度为k的单个子字符串中可能包含的最大元音字母数。

英文中的元音字母为 a e i o u。

```c
#define max(a,b) ( (b) > (a) ? (b) : (a))   // 利用宏定义处理简单函数
int maxVowels(char* s, int k) {
    int ans=0,vowel=0;              // ans为最大值，vowel为目前值
    for(int i=0; s[i]; i++) {
        // 1. 右指针的处理
        if( s[i] == 'a' || s[i] == 'e'  || s[i] == 'o' || s[i] == 'i' || s[i] == 'u') {
            vowel++;
        }
        if(i < k-1 ) {             // 控制窗口大小
            continue;
        }
        // 2.更新答案
        ans = max(ans,vowel);
        // 子串若均为元音则跳出循环
        if (ans == k) break;
        // 3.左指针的处理
        char left = s[i - k + 1];
        if(left == 'a' || left == 'e' || left == 'i' || left == 'o' || left == 'u') {
            vowel--;
        }
    }
    return ans;
}
```

要点解析：

- 首先，宏定义用来比大小，简化函数

- for循环当中的 s [ i ]  为 ' \ 0' 时，结束循环

- 下面这个if语句则是控制窗口大小，若窗口小于k则跳过后续逻辑，若窗口大小>=k则开始更新答案并处理窗口逻辑

  >if(i < k-1 ) {             // 控制窗口大小
  >            continue;
  >        }

<p align="right">完成时间：2025年3月12日09点25分</p>