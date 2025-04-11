# [定长滑动窗口(进阶题)](https://leetcode.cn/discuss/post/0viNMK/)
- [x] [3439.重新安排会议得到最多空余时间 I 1729](#[3439. 重新安排会议得到最多空余时间 I])
- [x] [2134.最少交换次数来组合所有的 1 II 1748](#2134-最少交换次数来组合所有的-1-ii)
- [ ] [1297.子串的最大出现次数 1748](#1297-子串的最大出现次数)
- [x] [2653.滑动子数组的美丽值 1786](#2653. 滑动子数组的美丽值)
- [ ] [1888.使二进制字符串字符交替的最少反转次数 2006](#1888-使二进制字符串字符交替的最少反转次数)
- [ ] [567.字符串的排列](#567-字符串的排列)
- [ ] [438.找到字符串中所有字母异位词](#438-找到字符串中所有字母异位词)
- [ ] [30.串联所有单词的子串](#30-串联所有单词的子串)
- [ ] [2156.查找给定哈希值的子串 2063](#2156-查找给定哈希值的子串)
- [ ] [2953.统计完全子字符串 2449](#2953-统计完全子字符串)
- [ ] [1016.子串能表示从 1 到 N 数字的二进制串 做到 O(∣s∣)](#1016-子串能表示从-1-到-n-数字的二进制串)
- [ ] 683.K 个关闭的灯泡（会员题）做到 O(n)
- [ ] 2067.等计数子串的数量（会员题）
- [ ] 2524.子数组的最大频率分数（会员题）


## [3439. 重新安排会议得到最多空余时间 I](https://leetcode.cn/problems/reschedule-meetings-for-maximum-free-time-i/)

给你一个整数 `eventTime` 表示一个活动的总时长，这个活动开始于 `t = 0` ，结束于 `t = eventTime` 。

同时给你两个长度为 `n` 的整数数组 `startTime` 和 `endTime` 。它们表示这次活动中 `n` 个时间 **没有重叠** 的会议，其中第 `i` 个会议的时间为 `[startTime[i], endTime[i]]` 。

你可以重新安排 **至多** `k` 个会议，安排的规则是将会议时间平移，且保持原来的 **会议时长** ，你的目的是移动会议后 **最大化** 相邻两个会议之间的 **最长** 连续空余时间。

移动前后所有会议之间的 **相对** 顺序需要保持不变，而且会议时间也需要保持互不重叠。

请你返回重新安排会议以后，可以得到的 **最大** 空余时间。

**注意**，会议 **不能** 安排到整个活动的时间以外。

```c
#define max(a,b) a>b?a:b
int maxFreeTime(int eventTime, int k, int* startTime, int startTimeSize, int* endTime, int endTimeSize) {
    // 空闲时间计算，startTime[0]+(startTime[i]-endTime[i-1])
    int free_Time[startTimeSize + 1];
    free_Time[startTimeSize] = eventTime-endTime[startTimeSize-1];
    for(int i = 0; i < startTimeSize; i++) {
        if(i == 0)free_Time[i] = startTime[i];
        else {
        free_Time[i] = startTime[i] - endTime[i-1];
        }
    }
    // 开始滑动
    int ans=0, Max_free_time = 0,windows=k+1;                // windows为实际窗口大小
    if(startTimeSize < windows){
       for(int i = 0; i < startTimeSize + 1; i++) {         
        ans += free_Time[i];
       }
    }else {
    for(int right = 0; right < startTimeSize+1; right++) {  // 本题的空闲时间数组比startTimeSize大一，请注意
        Max_free_time += free_Time[right];
        if(right < windows - 1 ) continue;
        ans=max(Max_free_time,ans);
        int left = right -windows + 1;
        Max_free_time -= free_Time[left];
    }
    }
    return ans;
}
```

要点解析:

- <font color ="Red">问题转换，至多k个会议，就是有k个挡板，即共有k+1个窗口，然后我们通过提前计算出空闲时间的数组，然后再空闲数组上进行滑动窗口找到答案</font>

<p align="right">完成时间：2025年3月17日17点37分</p>

## [2134. 最少交换次数来组合所有的 1 II](https://leetcode.cn/problems/minimum-swaps-to-group-all-1s-together-ii/)

**交换** 定义为选中一个数组中的两个 **互不相同** 的位置并交换二者的值。

**环形** 数组是一个数组，可以认为 **第一个** 元素和 **最后一个** 元素 **相邻** 。

给你一个 **二进制环形** 数组 `nums` ，返回在 **任意位置** 将数组中的所有 `1` 聚集在一起需要的最少交换次数。

```c
#define MAX(a,b) a>b?a:b
int minSwaps(int* nums, int numsSize) {
    int windows=0;
    // 找到定长窗口的长度
    for(int i=0; i<numsSize; i++) {
        if(nums[i] == 1) windows++;                         // 找到总共有多少个1，即为窗口大小或者数组总和
    }
    int ans = 0, sum=0;
    // numsSize+winodws可以让right多往右边划一点，然后再让滑动的时候对numsSize取余就可以得到一个首尾相连的窗口
    for(int right = 0; right < numsSize + windows; right++) {
        sum += nums[right % numsSize];            
        if(right < windows - 1) continue;
        ans=MAX(ans,sum);
        int left = right - windows + 1;
        sum -= nums[left % numsSize];
    }
    return windows-ans;
}
```

要点解析：

- 有关于这种首尾相连的数据结构，通常使用对<font color="Red">长度进行一个取余操作</font>从而实现首尾相连

- 套公式

<p align="right">完成时间：2025年3月18日9点14分</p>

## [1297. 子串的最大出现次数](https://leetcode.cn/problems/maximum-number-of-occurrences-of-a-substring/)

给你一个字符串 `s` ，请你返回满足以下条件且出现次数最大的 **任意** 子串的出现次数：

- 子串中不同字母的数目必须小于等于 `maxLetters` 。
- 子串的长度必须大于等于 `minSize` 且小于等于 `maxSize` 。

```c

```

## [2653. 滑动子数组的美丽值](https://leetcode.cn/problems/sliding-subarray-beauty/)

给你一个长度为 `n` 的整数数组 `nums` ，请你求出每个长度为 `k` 的子数组的 **美丽值** 。

一个子数组的 **美丽值** 定义为：如果子数组中第 `x` **小整数** 是 **负数** ，那么美丽值为第 `x` 小的数，否则美丽值为 `0` 。

请你返回一个包含 `n - k + 1` 个整数的数组，**依次** 表示数组中从第一个下标开始，每个长度为 `k` 的子数组的 **美丽值** 。

- 子数组指的是数组中一段连续 **非空** 的元素序列。

```c
int* getSubarrayBeauty(int* nums, int numsSize, int k, int x, int* returnSize) {
    int *arr = (int*)malloc((numsSize - k + 1)*sizeof(int));   // 存储美丽值
    int *temp = (int*)calloc(sizeof(int),101);                       // 统计元素的频率
    *returnSize = numsSize - k + 1;
    int i=0,j=0,sz;
    // 处理第一个窗口的元素
    for(i = 0; i < k ;i++) {
        temp[nums[i] + 50]++;
    }
    for(i = 0; i + k <= numsSize; i++) {
        if(i != 0 ) {
            temp[nums[i - 1] + 50]--;                              // 左边的出去
            temp[nums[i + k - 1] + 50]++;                     // 右边的进来
        }
        for(j = 0, sz = x; j < 100; j++) {                        // 每次均会重置窗口大小
            if(temp[j] == 0) continue;
            sz -= temp[j];
            if(sz <= 0) break;
        }
        if( j - 50 <0) arr[i] = j-50;
        else arr[i] = 0;
    }
    free(temp);
    return arr;
}
```

要点解析：

- 定义两个数组，一个存储美丽值（答案数组），一个统计元素的频率，其中统计元素频率的数组会一直更新，每次滑动窗口都会将出去窗口-1，进入窗口+1，并且<font color ="red">该数组会将元素+50作为index来用</font>
- j的for循环每次都会从左往右删除窗口，temp[j]==0说明窗口里面没这个数，跳过这个数，一直这样找到第x个，那么就是第x小的数字（毕竟这个数组以元素为index，自然是有序的），每次循环后下一次都会被恢复

>   for(j = 0, sz = x; j < 100; j++) {
>            if(temp[j] == 0) continue;
>            sz -= temp[j];
>            if(sz <= 0) break;
>        }

- j-50<0即是负数也就是美丽值本身，因为我们加了50作为偏移量，因此要减去，j-50>0则说明为正数，美丽值为0

<p align="right">完成时间：2025年3月19日21点39分</p>

## [1888. 使二进制字符串字符交替的最少反转次数](https://leetcode.cn/problems/minimum-number-of-flips-to-make-the-binary-string-alternating/)

给你一个二进制字符串 `s` 。你可以按任意顺序执行以下两种操作任意次：

- **类型 1 ：删除** 字符串 `s` 的第一个字符并将它 **添加** 到字符串结尾。
- **类型 2 ：选择** 字符串 `s` 中任意一个字符并将该字符 **反转** ，也就是如果值为 `'0'` ，则反转得到 `'1'` ，反之亦然。

请你返回使 `s` 变成 **交替** 字符串的前提下， **类型 2** 的 **最少** 操作次数 。

我们称一个字符串是 **交替** 的，需要满足任意相邻字符都不同。

- 比方说，字符串 `"010"` 和 `"1010"` 都是交替的，但是字符串 `"0100"` 不是。

```c

```



## [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)



## [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

## [30. 串联所有单词的子串](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/)

## [2156. 查找给定哈希值的子串](https://leetcode.cn/problems/find-substring-with-given-hash-value/)

## [2953. 统计完全子字符串](https://leetcode.cn/problems/count-complete-substrings/)

## [1016. 子串能表示从 1 到 N 数字的二进制串](https://leetcode.cn/problems/binary-string-with-substrings-representing-1-to-n/)

