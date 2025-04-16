# [不定长滑动窗口：求子数组个数（越长越合法）](https://leetcode.cn/discuss/post/0viNMK/)

- [x] [1358.包含所有三种字符的子字符串数目 1646](#1358-包含所有三种字符的子字符串数目)
- [x] [2962.统计最大元素出现至少 K 次的子数组 1701](#2962-统计最大元素出现至少-k-次的子数组)
- [x] [3325.字符至少出现 K 次的子字符串 I 做到 O(n)](#3325-字符至少出现-k-次的子字符串-i)
- [x] [2799.统计完全子数组的数目 做到 O(n)](#2799-统计完全子数组的数目)
- [ ] [2537.统计好子数组的数目 1892](#2537-统计好子数组的数目)
- [x] [3298.统计重新排列后包含另一个字符串的子字符串数目 II 1909 同 76 题](#3298-统计重新排列后包含另一个字符串的子字符串数目-ii)
- [ ] 2495.乘积为偶数的子数组数（会员题）

### <font color = "Red">解题方法：</font>

### <font color = "Red">一般要写 ans += left。</font>

### <font color = "Red">滑动窗口的内层循环结束时，右端点固定在 right，左端点在 0,1,2,…,left−1 的所有子数组（子串）都是合法的，这一共有 left 个。</font>

##  [1358. 包含所有三种字符的子字符串数目](https://leetcode.cn/problems/number-of-substrings-containing-all-three-characters/)

给你一个字符串 `s` ，它只包含三种字符 a, b 和 c 。

请你返回 a，b 和 c 都 **至少** 出现过一次的子字符串数目。

```c
int numberOfSubstrings(char* s) {
    int cnt[128] = {0}; // 分别记录abc的出现次数
    int kind = 0,sum = 0; // kind == 3则为包含所有字符
    int left = 0;
    for(int right = 0; s[right]; right++) {
        if(cnt[s[right]]++ == 0)kind++;
        while(kind == 3) {
            if(--cnt[s[left++]] == 0)kind--;
        }
        sum +=left;
    }
    return sum;
}
```

要点解析：

- 最重要的莫过于left的使用，滑动窗口的内层循环结束时，<font color = "Red">右端点**固定**在 *right*，左端点在 0,1,2,…,*left*−1 的所有子串都是合法的，这一共有 *left* 个，</font>加入答案。

- 依旧是熟悉的简易hash映射配合kind 的操作

<p align="right">完成时间：2025年4月12日17点05分</p>

## [2962. 统计最大元素出现至少 K 次的子数组](https://leetcode.cn/problems/count-subarrays-where-max-element-appears-at-least-k-times/)

给你一个整数数组 `nums` 和一个 **正整数** `k` 。

请你统计有多少满足 「 `nums` 中的 **最大** 元素」至少出现 `k` 次的子数组，并返回满足这一条件的子数组的数目。

子数组是数组中的一个连续元素序列。

```c
long long countSubarrays(int* nums, int numsSize, int k) {
    int left = 0, max = 0,curr = 0, idx = 0;
    long long ans = 0;
    for(int i = 0; i < numsSize; i++) {
        if(nums[i] > max) {
            max = nums[i];
            idx = i;
            curr = 1;
        }
        if(nums[i] == max) {
            curr++;
        }
    }
    if(curr < k) return 0;
    curr = 0;
    for(int right = idx;right < numsSize; right++){
        if(nums[right] == max)curr++;
        while(curr >= k){
            if(nums[left++] == max)curr--;
        }
        ans+=left;
    }
    return ans;
}
```

要点解析：

- 第一次循环获得最大值以及最大值出现的第一次位置，并且统计最大值有多少个，如果少于K个则直接返回0；
- 依旧是对于left的应用，ans += left，滑动窗口的内层循环结束时，<font color = "Red">右端点**固定**在 *right*，左端点在 0,1,2,…,*left*−1 的所有子串都是合法的，这一共有 *left* 个</font>

<p align="right">完成时间：2025年4月14日10点14分</p>

## [3325. 字符至少出现 K 次的子字符串 I](https://leetcode.cn/problems/count-substrings-with-k-frequency-characters-i/)

给你一个字符串 `s` 和一个整数 `k`，在 `s` 的所有子字符串中，请你统计并返回 **至少有一个** 字符 **至少出现** `k` 次的子字符串总数。

**子字符串** 是字符串中的一个连续、 **非空** 的字符序列。

```c
int numberOfSubstrings(char* s, int k) {
    int ans = 0, curr = 0,left = 0; // curr代表这个窗口中大于2的字符种类数
    int cnt[128] ={0};
    for(int right = 0; s[right]; right++) {
        if(++cnt[s[right]] == k)curr++;
        while(curr >= 1 ){
            if(cnt[s[left++]]-- == k)curr--;
        }
        ans += left;
    }
    return ans;
}
```

要点解析：

- 依旧是对于left的应用，没哈其他特殊的地方

  <p align="right">完成时间：2025年4月14日10点39分</p>

## [2799. 统计完全子数组的数目](https://leetcode.cn/problems/count-complete-subarrays-in-an-array/)

给你一个由 **正** 整数组成的数组 `nums` 。

如果数组中的某个子数组满足下述条件，则称之为 **完全子数组** ：

- 子数组中 **不同** 元素的数目等于整个数组不同元素的数目。

返回数组中 **完全子数组** 的数目。

**子数组** 是数组中的一个连续非空序列。

```c
int countCompleteSubarrays(int* nums, int numsSize) {
    int ans = 0, left = 0, curr = 0,sum_kind = 0; // curr 指的是当前窗口不同元素数
    // sum_kind指的是整个数组不同元素的个数
    int cnt[2001] = {0};
    for(int i = 0; i < numsSize; i++) {
        if(++cnt[nums[i]] == 1) sum_kind++;
    }
    memset(cnt, 0, sizeof(cnt));
    for(int right = 0; right < numsSize; right++) {
     if(++cnt[nums[right]] == 1)curr++;
     while(curr == sum_kind) {
        if(--cnt[nums[left++]] == 0)curr--;
    }
     ans += left;
    }
    return ans;
}
```

要点解析：

- 依旧使用哈希映射，记得配合memset将cnt数组全部归零
- ans+=left老样子，内层循环结束的时候使用

<p align="right">完成时间：2025年4月14日10点56分</p>

## [2537. 统计好子数组的数目](https://leetcode.cn/problems/count-the-number-of-good-subarrays/)

```c
long long countGood(int* nums, int numsSize, int k) {
    long long ans = 0;
    int cnt[10] = {0};
    int left = 0, curr = 0; // curr指的是当前窗口有的配对数
    for(int right = 0; right < numsSize; right++) {
        curr += cnt[nums[right]]++;  // 每出现一个X，那么这个X就会和之前的X进行配对
        // 所以是curr += cnt++,那么同样的维护左指针的时候需要curr -= --cnt;
        while(curr >= k) {
            curr -= --cnt[nums[left++]];
        }
        ans += left;
    }
    return ans;
}
```

要点解析：

- 暂时先这样吧，题目给的数字太大了，不能使用这种建议哈希进行处理，得真正使用hash表，那么先放在这里，等后续学完c++再来补充

  <p align="right">完成时间：2025年4月14日21点14分</p>

## [3298. 统计重新排列后包含另一个字符串的子字符串数目 II](https://leetcode.cn/problems/count-substrings-that-can-be-rearranged-to-contain-a-string-ii/)

给你两个字符串 `word1` 和 `word2` 。

如果一个字符串 `x` 重新排列后，`word2` 是重排字符串的 前缀 ，那么我们称字符串 `x` 是 **合法的** 。

请你返回 `word1` 中 **合法** 子字符串 的数目。

**注意** ，这个问题中的内存限制比其他题目要 **小** ，所以你 **必须** 实现一个线性复杂度的解法。

```c
long long validSubstringCount(char* word1, char* word2) {
    int cnt[128] = {0};
    long long ans = 0;
    int curr = 0, left = 0, sum = 0;
    for(int i = 0; word2[i];i++) {
        if(++cnt[word2[i]] == 1)sum++;
    }
    for(int right = 0; word1[right]; right++) {
        if(--cnt[word1[right]] == 0)curr++;
        while(curr == sum){
            if(++cnt[word1[left++]] == 1)curr--;
        }
        ans += left;
    }
    return ans;
}
```

~~ps:莫非我真的成为滑动窗口高手了（不是），哇哈哈？hard题目，一次ac~~

要点解析：

- 依旧是简单哈希映射，直接秒杀hard
- 精髓之处依旧是left的使用

<p align="right">完成时间：2025年4月14日21点22分</p>