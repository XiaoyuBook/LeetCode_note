# [定长滑动窗口(基础题)](https://leetcode.cn/discuss/post/0viNMK/)

## [1456.定长字串中元音的最大数目- 力扣（LeetCode）]([1456. 定长子串中元音的最大数目 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/description/))

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

## [643. 子数组最大平均数 I - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-average-subarray-i/description/)

给你一个由 `n` 个元素组成的整数数组 `nums` 和一个整数 `k` 。

请你找出平均数最大且 **长度为 `k`** 的连续子数组，并输出该最大平均数。

任何误差小于 `10-5` 的答案都将被视为正确答案。

```c
#define max(a, b) ((b) > (a) ? (b) : (a))
double findMaxAverage(int* nums, int numsSize, int k) {
    int s = 0;                           // 当前窗口的元素和
    int max_sum = INT_MIN;                     // 最大值
    for( int right = 0 ; right < numsSize ; right++ ) {
        s += nums[right];
        if( right < k - 1 ) {
            continue;
        }
        max_sum = max(max_sum, s);
        int left = ( right - k + 1);
        s -= nums[right - k + 1];
    }
    return (double) max_sum / k;
}
```

要点分析：

- 其中一开始的最大值`max_sum`，我们需要将它赋值为<font color='red'>INT_MIN</font>,如若简单的赋值为0，出现负数情况，则可能会失败
- `nums`为一个整数数组，因此，我们可以在最后使用一个double的强制类型转换以达到结果

<p align="right">完成时间：2025年3月12日10点36分</p>

## [1343. 大小为 K 且平均值大于等于阈值的子数组数目](https://leetcode.cn/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/)

给你一个整数数组 `arr` 和两个整数 `k` 和 `threshold` 。

请你返回长度为 `k` 且平均值大于等于 `threshold` 的子数组数目。

```c
int numOfSubarrays(int* arr, int arrSize, int k, int threshold) {
    int right = 0;
    int compare=threshold*k;        //平均值转化为和
    int sum = 0;
    int num = 0;                    
    for(right=0; right<arrSize; right++) {
        sum += arr[right];
        if(right < k-1) continue;
        if(sum >= compare)num++;
        int left = right - k + 1;
        sum -=arr[left];
    }
    return num;
}
```

要点解析：

- 牢记三步骤：进，更新，出（for循环）

- 最重要的依旧是理解<font color ="Red">if(right < k-1) continue;</font>

<p align="right">完成时间：2025年3月12日21点43分</p>

## [2090. 半径为 k 的子数组平均值](https://leetcode.cn/problems/k-radius-subarray-averages/)

给你一个下标从 **0** 开始的数组 `nums` ，数组中有 `n` 个整数，另给你一个整数 `k` 。

**半径为 k 的子数组平均值** 是指：`nums` 中一个以下标 `i` 为 **中心** 且 **半径** 为 `k` 的子数组中所有元素的平均值，即下标在 `i - k` 和 `i + k` 范围（**含** `i - k` 和 `i + k`）内所有元素的平均值。如果在下标 `i` 前或后不足 `k` 个元素，那么 **半径为 k 的子数组平均值** 是 `-1` 。

构建并返回一个长度为 `n` 的数组 `avgs` ，其中 `avgs[i]` 是以下标 `i` 为中心的子数组的 **半径为 k 的子数组平均值** 。

`x` 个元素的 **平均值** 是 `x` 个元素相加之和除以 `x` ，此时使用截断式 **整数除法** ，即需要去掉结果的小数部分。

- 例如，四个元素 `2`、`3`、`1` 和 `5` 的平均值是 `(2 + 3 + 1 + 5) / 4 = 11 / 4 = 2.75`，截断后得到 `2` 

```c
int* getAverages(int* nums, int numsSize, int k, int* returnSize) {
    *returnSize = numsSize;
    int* ave = malloc(numsSize * sizeof(int));
    memset(ave, -1, numsSize * sizeof(int)); // 初始化为 -1
    long long sum = 0;
    int num = 0; // 用于记录窗口的起始位置
    for (int right = 0; right < numsSize; right++) {
        sum += nums[right];
        if (right < k * 2) {
            continue;
        }
        ave[right - k] = sum / (k * 2 + 1);
        sum -= nums[right - k * 2];
    }
    return ave;
}
```

要点解析：

-  <font color="Red">memset(ave, -1, numsSize * sizeof(int))</font>; 将数组全部赋为-1，即代替for循环遍历

  ><font color ="Red">memset(ave, -1, numsSize * sizeof(int));</font>
  >
  >1. **`ave`**：是结果数组的起始地址。
  >2. **`-1`**：是要设置的值。
  >3. **`numsSize * sizeof(int)`**：是要填充的字节数，即整个数组的大小。

- 无法确定数组大小一定要用malloc配合sizeof来处理，也需要记得最后用free释放空间
- 使用 `long long sum` 是为了避免在计算滑动窗口的和时发生整数溢出

<p align="right">完成时间：2025年3月12日22点07分</p>