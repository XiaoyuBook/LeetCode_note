# [不定长滑动窗口：求子数组个数（越短越合法）](https://leetcode.cn/discuss/post/0viNMK/)

- [x] [713.乘积小于 K 的子数组](#713-乘积小于-k-的子数组)
- [x] [3258.统计满足 K 约束的子字符串数量 I 做到 O(n)](#3258-统计满足-k-约束的子字符串数量-i)
- [x] [2302.统计得分小于 K 的子数组数目 1808](#2302-统计得分小于-k-的子数组数目)
- [x] [2762.不间断子数组 1940](#2762-不间断子数组)
- [x] [LCP 68. 美观的花束](#lcp-68-美观的花束)
- [ ] 2743.计算没有重复字符的子字符串数量（会员题）

<font color = "Red">解题方法：</font>

<font color = "Red">一般要写 ans += right - left + 1。</font>

<font color = "Red">滑动窗口的内层循环结束时，右端点固定在 right，左端点在 left,left+1,…,right 的所有子数组（子串）都是合法的，这一共有 right−left+1 个。</font>

## [713. 乘积小于 K 的子数组](https://leetcode.cn/problems/subarray-product-less-than-k/)

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回子数组内所有元素的乘积严格小于 `k` 的连续子数组的数目

```c
int numSubarrayProductLessThanK(int* nums, int numsSize, int k) {
    int left = 0, curr = 1, ans = 0;
    if(k <= 1)return 0;
    for(int right = 0; right < numsSize; right++) {
        curr *= nums[right];
        while(curr >= k) {
            curr /= nums[left++];
        }
        ans += right - left + 1;
    }
    return ans;
}
```

要点解析：

- 十分类似于越长越合法的解题方法，只是简单的从 ans += right；换成 ans += right - left + 1;即可
- 那么这题还要注意    if(k <= 1)return 0;，否则while将无限循环

<p align="right">完成时间：2025年4月14日21点43分</p>

## [3258. 统计满足 K 约束的子字符串数量 I](https://leetcode.cn/problems/count-substrings-that-satisfy-k-constraint-i/)

给你一个 **二进制** 字符串 `s` 和一个整数 `k`。

如果一个 **二进制字符串** 满足以下任一条件，则认为该字符串满足 **k 约束**：

- 字符串中 `0` 的数量最多为 `k`。
- 字符串中 `1` 的数量最多为 `k`。

返回一个整数，表示 `s` 的所有满足 **k 约束** 的子字符串的数量。

```c
int countKConstraintSubstrings(char* s, int k) {
    int ans = 0, left = 0;
    int cnt[2] = {0};
    for(int right = 0; s[right]; right++) {
        cnt[s[right]-'0']++;
        while(cnt[0] > k && cnt[1] > k) {
            cnt[s[left++]-'0']--;
        }
        ans += right - left + 1;
    }
    return ans;
}
```

~~ps:读题花了点时间，滑动窗口高手的陨落~~

要点解析：

- 依旧秒杀，但是要注意不能用s[right]直接作为索引，因为s是字符串所以要减去'0'，如果s是int数组则可以直接当做索引

<p align="right">完成时间：2025年4月14日21点56分</p>

## [2302. 统计得分小于 K 的子数组数目](https://leetcode.cn/problems/count-subarrays-with-score-less-than-k/)

一个数组的 **分数** 定义为数组之和 **乘以** 数组的长度。

- 比方说，`[1, 2, 3, 4, 5]` 的分数为 `(1 + 2 + 3 + 4 + 5) * 5 = 75` 。

给你一个正整数数组 `nums` 和一个整数 `k` ，请你返回 `nums` 中分数 **严格小于** `k` 的 **非空整数子数组数目**。

**子数组** 是数组中的一个连续元素序列。

```c
long long countSubarrays(int* nums, int numsSize, long long k) {
    int left = 0;
    long long ans = 0, curr = 0;
    for(int right = 0; right < numsSize; right++) {
        curr += nums[right];
        while(curr*(right - left + 1) >= k) {
            curr -= nums[left++];
        }
        ans += right - left + 1;
    }
    return ans;
}
```

要点解析：

- 我们要吧right - left + 1放进while条件里面，不要while外面用，导致乘了之后，维护left的时候还要除出去，麻烦

<p align="right">完成时间：2025年4月14日22点05分</p>

## [2762. 不间断子数组](https://leetcode.cn/problems/continuous-subarrays/)

给你一个下标从 **0** 开始的整数数组 `nums` 。`nums` 的一个子数组如果满足以下条件，那么它是 **不间断** 的：

- `i`，`i + 1` ，...，`j` 表示子数组中的下标。对于所有满足 `i <= i1, i2 <= j` 的下标对，都有 `0 <= |nums[i1] - nums[i2]| <= 2` 。

请你返回 **不间断** 子数组的总数目。

子数组是一个数组中一段连续 **非空** 的元素序列。

```c
long long continuousSubarrays(int* nums, int numsSize){
    long long ans = 0;
    int left = 0;
    int maxDeque[100005], minDeque[100005];
    int maxFront = 0, maxBack = -1;
    int minFront = 0, minBack = -1;
    for(int right = 0; right < numsSize; right++) {
        while(maxBack >= maxFront && nums[right] > nums[maxDeque[maxBack]]){
            maxBack--;
        }
        maxDeque[++maxBack] = right;
        while(minBack >= minFront && nums[right] < nums[minDeque[minBack]]){
            minBack--;
        }
        minDeque[++minBack] = right;
        
        
        
        while(nums[maxDeque[maxFront]] - nums[minDeque[minFront]] >2 ) {
            if(maxDeque[maxFront] == left)maxFront++;
            if(minDeque[minFront] == left)minFront++;
            left++;
        }
        ans += right - left + 1;
    }
    return ans;
}
```

要点解析：

- 构造队列，记录当前窗口中的最大值和最小值，一旦离开窗口则立马更新
- 依旧是ans += right - left + 1

<p align="right">完成时间：2025年4月15日21点37分</p>

## [LCP 68. 美观的花束](https://leetcode.cn/problems/1GxJYY/)

力扣嘉年华的花店中从左至右摆放了一排鲜花，记录于整型一维矩阵 `flowers` 中每个数字表示该位置所种鲜花的品种编号。你可以选择一段区间的鲜花做成插花，且不能丢弃。 在你选择的插花中，如果每一品种的鲜花数量都不超过 `cnt` 朵，那么我们认为这束插花是 「美观的」。

> - 例如：`[5,5,5,6,6]` 中品种为 `5` 的花有 `3` 朵， 品种为 `6` 的花有 `2` 朵，**每一品种** 的数量均不超过 `3`

请返回在这一排鲜花中，共有多少种可选择的区间，使得插花是「美观的」。

**注意：**

- 答案需要以 `1e9 + 7 (1000000007)` 为底取模，如：计算初始结果为：`1000000008`，请返回 `1`

```c
int beautifulBouquet(int* flowers, int flowersSize, int cnt){
    int left = 0, ans = 0;
    int cnt_arr[100001] = {0};
    for(int right = 0; right < flowersSize; right++) {
        cnt_arr[flowers[right]]++;
        while(cnt_arr[flowers[right]] > cnt && left <= right) {
            cnt_arr[flowers[left++]]--;
        }
        ans += right - left + 1;
    }
    ans %=1000000007;
    return ans;
}
```

要点解析：

- 他是让每种花数量均不超过3，因此我们需要一个cnt数组统计每种花的数量，每次循环都判断一下是否超过规定数量，如果超过则更新左窗口

<p align="right">完成时间：2025年4月16日9点08分</p>