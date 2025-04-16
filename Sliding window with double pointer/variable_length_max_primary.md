# [不定长滑动窗口：求最大/最长(基础题)](https://leetcode.cn/discuss/post/0viNMK/)

- [x] [3.无重复字符的最长子串](# 3. 无重复字符的最长子串)
- [x] [3090.每个字符最多出现两次的最长子字符串 1329](# 3090.每个字符最多出现两次的最长子字符串)
- [x] [1493.删掉一个元素以后全为 1 的最长子数组 1423](#1493-删掉一个元素以后全为-1-的最长子数组)
- [x] [1208.尽可能使字符串相等 1497](#1208-尽可能使字符串相等)
- [x] [904.水果成篮 1516](#904-水果成篮)
- [x] [1695.删除子数组的最大得分 1529](#1695-删除子数组的最大得分)
- [x] [2958.最多 K 个重复元素的最长子数组 1535](#2958-最多-k-个重复元素的最长子数组)
- [x] [2024.考试的最大困扰度 1643](#2024-考试的最大困扰度)
- [x] [1004.最大连续 1 的个数 III 1656](#1004-最大连续1的个数-iii)
- [x] [1658.将 x 减到 0 的最小操作数 1817](#1658-将-x-减到-0-的最小操作数)

## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长 子串** 的长度。

```c
#define MAX(a, b) ((a) > (b)) ? (a) : (b)
int lengthOfLongestSubstring(char* s) {
    int ans = 0, left = 0;
    int cnt[128] = {0};
    for(int right = 0; s[right]; right++) {
        char c = s[right];
        cnt[c]++;
        while(cnt[c]>1) {
            cnt[s[left]]--;
            left++;
        }
        ans = MAX(ans, right - left + 1);
    }
    return ans;
}
```

要点解析：

- 创建一个cnt数组用来记录当前子串中各个字符出现的字数，用其ASCII作为其index
- cnt[c]>1则说明重复了，因此要将最左侧元素移除

<p align="right">完成时间：2025年3月24日11点35分</p>

## [3090. 每个字符最多出现两次的最长子字符串](https://leetcode.cn/problems/maximum-length-substring-with-two-occurrences/)

给你一个字符串 `s` ，请找出满足每个字符最多出现两次的最长子字符串，并返回该子字符串的 **最大** 长度。

```c
#define MAX(a, b) (a) > (b) ? (a) : (b)
int maximumLengthSubstring(char* s) {
    int left = 0, ans = 0;
    int cnt[128] = {0};
    for(int right = 0; s[right]; right++) {
        char c = s[right];
        cnt[c]++;
        while(cnt[c] > 2) {
            cnt[s[left]]--;
            left++;
        }
        ans = MAX(ans, right - left + 1);
    }
    return ans;
}
```

要点解析：

- 同上一题，仅需将cnt[c]大于1改成大于2即可


<p align="right">完成时间：2025年3月24日11点43分</p>

## [1493. 删掉一个元素以后全为 1 的最长子数组](https://leetcode.cn/problems/longest-subarray-of-1s-after-deleting-one-element/)

给你一个二进制数组 `nums` ，你需要从中删掉一个元素。

请你在删掉元素的结果数组中，返回最长的且只包含 1 的非空子数组的长度。

如果不存在这样的子数组，请返回 0 。

### 非滑动做法

```c
#define MAX(a,b) (a) > (b) ? (a) : (b)
int longestSubarray(int* nums, int numsSize) {
    int left = 0, right = 0;
    int max = 0;
    for(int i = 0; i < numsSize; i++) {
        if(nums[i] == 1) right++;
        else {
            left = right;
            right = 0;
        }
        max =MAX(max,left+right);
    }
    if(right  == numsSize) max-=1;
    return max;
}
```

要点解析：

- 遇到下一个0前，统计 右边的个数
- 碰到下一个0时，之前0的**右边** 就成了 现在0的**左边**。
- 最后需要判断特殊情况，数组全1。

<p align="right">完成时间：2025年3月24日10点30分</p>

### 滑动窗口做法

```c
#define MAX(a,b) (a) > (b) ? (a) : (b)
int longestSubarray(int* nums, int numsSize) {
    int idx =-1, ans = 0;                                  
    int right =0, left = 0;
    for(right = 0; right < numsSize; right++) {
        if(nums[right] == 0) {
            left = idx +1;
            idx = right;
        }
        ans = MAX(ans, right - left);
    }
    return ans;
}
```

要点解析：

- idx的作用：标记0的位置，一开始将idx设为-1，极为巧妙，如下

> 如若，第一个为0，那么idx会变为right即0，然后正常运作
>
> 如若，没有0，那么right - left 正好也为我们要的答案，毕竟right的最大值本就是长度-1
>
> 那么正常情况的话，left就为idx+1=0，然后idx在更新为right

- 解题关键在于，最长子串中只能有一个0
>- 只要记录上一个 0 的出现位置就好，如果后面再看到 0，就将 left 右移一位，跳过上次看到 0 的位置。
>
>- 如果全部都是 1 也没关系，因为我们的滑动窗口只是记录最多含一个 0 的字串，有没有 0 都是正确的。

<p align="right">完成时间：2025年3月24日11点27分</p>

## [1208. 尽可能使字符串相等](https://leetcode.cn/problems/get-equal-substrings-within-budget/)

给你两个长度相同的字符串，`s` 和 `t`。

将 `s` 中的第 `i` 个字符变到 `t` 中的第 `i` 个字符需要 `|s[i] - t[i]|` 的开销（开销可能为 0），也就是两个字符的 ASCII 码值的差的绝对值。

用于变更字符串的最大预算是 `maxCost`。在转化字符串时，总开销应当小于等于该预算，这也意味着字符串的转化可能是不完全的。

如果你可以将 `s` 的子字符串转化为它在 `t` 中对应的子字符串，则返回可以转化的最大长度。

如果 `s` 中没有子字符串可以转化成 `t` 中对应的子字符串，则返回 `0`。

```c
#define MAX(a,b) (a) > (b) ? (a) : (b)
#define gap(a,b) (int)(a) - (int)(b)
int equalSubstring(char* s, char* t, int maxCost) {
    int ans = 0, max = 0;
    int right = 0, left = 0,len = strlen(s);
    for(int right = 0; right < len; right++) {
        int temp = abs(gap(s[right], t[right]));
        max +=temp;
        while(max > maxCost) {
            max -= abs(gap(s[left], t[left]));
            left++;
        }
         ans = MAX(right - left + 1 ,ans);
       
    }
    return ans;
}
```

要点解析：

- 长度不固定，左右指针处于同一起点，处理方式如下

> max > maxCost，则左指针开始右移且max减去左指针的差值，直至max < maxCost，右指针才可以开始右移
>
> max <= maxCost，正常右移直至max > maxCost

- 最后返回最长子串的长度，即right - left +1 ，和以前的记录对比

<p align="right">完成时间：2025年3月28日21时15分</p>

## [904. 水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)

你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 `fruits` 表示，其中 `fruits[i]` 是第 `i` 棵树上的水果 **种类** 。

你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：

- 你只有 **两个** 篮子，并且每个篮子只能装 **单一类型** 的水果。每个篮子能够装的水果总量没有限制。
- 你可以选择任意一棵树开始采摘，你必须从 **每棵** 树（包括开始采摘的树）上 **恰好摘一个水果** 。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。
- 一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。

给你一个整数数组 `fruits` ，返回你可以收集的水果的 **最大** 数目。

```c
#define MAX(a,b) (a) > (b) ? (a) : (b)
int totalFruit(int* fruits, int fruitsSize){
    int ans[100000] = {0};
    int max = INT_MIN;
    int kind = 0;
    int left = 0;
    for(int right = 0; right < fruitsSize; right++) {
        if(ans[fruits[right]] == 0) {
            kind++;
        }
        ans[fruits[right]]++;
        while(kind > 2) {
            ans[fruits[left]]--;
            if(ans[fruits[left]] == 0) kind--;
            left++;
        }
        max = MAX(max,right - left + 1);
    }
    return max;
}
```

要点解析：

- 一定要学会使用哈希映射，多使用类似于`ans[fruits[right]]`的使用方法

<p align="right">完成时间：2025年4月6日9时22分</p>

## [1695. 删除子数组的最大得分](https://leetcode.cn/problems/maximum-erasure-value/)

给你一个正整数数组 `nums` ，请你从中删除一个含有 **若干不同元素** 的子数组**。**删除子数组的 **得分** 就是子数组各元素之 **和** 。

返回 **只删除一个** 子数组可获得的 **最大得分** *。*

如果数组 `b` 是数组 `a` 的一个连续子序列，即如果它等于 `a[l],a[l+1],...,a[r]` ，那么它就是 `a` 的一个子数组。

```c
int maximumUniqueSubarray(int* nums, int numsSize) {
    int max = 0, ans = 0;
    int left = 0;
    int kind[10001]={0};
    for(int right = 0; right < numsSize; right++) {
        while(kind[nums[right]] == 1){
            max -= nums[left];
            kind[nums[left]]=0;
            left++;
        }
        max += nums[right];
        kind[nums[right]] =1;
        if(max > ans ) ans = max;
    }
    return ans;
}
```

要点解析：

- 类似于上一题，不过我选择直接处理有重复元素的情况，这样就省去一次if判断。

<p align="right">完成时间：2025年4月6日21时37分</p>

## [2958. 最多 K 个重复元素的最长子数组](https://leetcode.cn/problems/length-of-longest-subarray-with-at-most-k-frequency/)

给你一个整数数组 `nums` 和一个整数 `k` 。

一个元素 `x` 在数组中的 **频率** 指的是它在数组中的出现次数。

如果一个数组中所有元素的频率都 **小于等于** `k` ，那么我们称这个数组是 **好** 数组。

请你返回 `nums` 中 **最长好** 子数组的长度。

**子数组** 指的是一个数组中一段连续非空的元素序列。

```c
#define MAX(a,b) (a) > (b) ? (a) : (b)
#define capacity 1000000004

int maxSubarrayLength(int* nums, int numsSize, int k) {
    int ans = 0;
    int left = 0;
     int* kind=(int*)malloc(sizeof(int)*capacity);
    memset(kind,0,sizeof(int)*capacity);
    for(int right = 0; right < numsSize; right++) {
        kind[nums[right]] ++;
        while(kind[nums[right]] > k){
            kind[nums[left]]--;
            left++;
        }
        ans = MAX(right - left + 1, ans);
    }
    return ans;
}
```

- 可以使用哈希表映射解决，但是目前c语言得手搓hashmap有点麻烦，等学到c++后再用，那么本题与上题类似

<p align="right">完成时间：2025年4月8日21时25分</p>

## [2024. 考试的最大困扰度](https://leetcode.cn/problems/maximize-the-confusion-of-an-exam/)

一位老师正在出一场由 `n` 道判断题构成的考试，每道题的答案为 true （用 `'T'` 表示）或者 false （用 `'F'` 表示）。老师想增加学生对自己做出答案的不确定性，方法是 **最大化** 有 **连续相同** 结果的题数。（也就是连续出现 true 或者连续出现 false）。

给你一个字符串 `answerKey` ，其中 `answerKey[i]` 是第 `i` 个问题的正确结果。除此以外，还给你一个整数 `k` ，表示你能进行以下操作的最多次数：

- 每次操作中，将问题的正确答案改为 `'T'` 或者 `'F'` （也就是将 `answerKey[i]` 改为 `'T'` 或者 `'F'` ）。

请你返回在不超过 `k` 次操作的情况下，**最大** 连续 `'T'` 或者 `'F'` 的数目。

```c
#define MAX(a,b) (a) > (b) ? (a) : (b)
int maxConsecutiveAnswers(char* answerKey, int k) {
    int len = strlen(answerKey);
    int left = 0;
    int anst = 0, ansf=0;
    int cur = 0;
    int count = 0;
    for(int right = 0; right < len; right++) {
        cur++;
        if(answerKey[right % len] == 'F') {
            count++;
        }
        while( count > k ) {
            if(answerKey[left] == 'F') {
                count--;
            }
            cur--;
            left++;
        }
        anst = MAX(cur,anst);
    }
    cur = 0;
    count = 0;
    left = 0;
    for(int right = 0; right < len; right++) {
        cur++;
        if(answerKey[right % len] == 'T') {
            count++;
        }
        while( count > k ) {
            if(answerKey[left] == 'T') {
                count--;
            }
            cur--;
            left++;
        }
        ansf = MAX(cur,ansf);
    }
    int ans = MAX(anst,ansf);
    return ans;
}
```

要点解析：

- 遍历两边，分别得到窗口中最多的T和最多的F，两个当中取最大即使我们所要的答案

<p align="right">完成时间：2025年4月8日21时52分</p>

## [1004. 最大连续1的个数 III](https://leetcode.cn/problems/max-consecutive-ones-iii/)

给定一个二进制数组 `nums` 和一个整数 `k`，假设最多可以翻转 `k` 个 `0` ，则返回执行操作后 *数组中连续 `1` 的最大个数* 。

```c
#define MAX(a,b) (a) > (b) ? (a) : (b) 
int longestOnes(int* nums, int numsSize, int k) {
    int curr = 0, ans = 0;
    int count = 0;
    int left = 0;
    for(int right = 0; right < numsSize; right++) {
        curr++;
        if(nums[right] == 0) {
            count++;
        }
        while(count > k) {
            if(nums[left] == 0) {
                count--;
            }
            curr--;
            left++;
        }
        ans = MAX(ans, curr);
    }
    return ans;
}
```

要点解析：

- 本题居然是上题的简化版，不知道为何要这样放置题单

<p align="right">完成时间：2025年4月8日22时03分</p>

## [1658. 将 x 减到 0 的最小操作数](https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/)

给你一个整数数组 `nums` 和一个整数 `x` 。每一次操作时，你应当移除数组 `nums` 最左边或最右边的元素，然后从 `x` 中减去该元素的值。请注意，需要 **修改** 数组以供接下来的操作使用。

如果可以将 `x` **恰好** 减到 `0` ，返回 **最小操作数** ；否则，返回 `-1` 。

```c
#define MAX(a,b) (a) > (b) ? (a) : (b) 
int minOperations(int* nums, int numsSize, int x) {
    int sum = 0;
    for(int i = 0; i < numsSize; i++) {
        sum += nums[i];
    }
    if(sum == x )return numsSize;
    if(sum < x) return -1;
    sum -= x; // 这个就是我们要求的窗口值
    int curr = 0,ans = 0, left = 0,count = 0;
    for(int right = 0; right < numsSize; right++) {
        curr += nums[right];
        count++;
        while(curr > sum) {
            curr -= nums[left];
            left++;
            count--;
        }
        if(curr == sum)ans = MAX(ans,count);
    }
    if(ans == 0) return -1;
    return numsSize-ans;
}
```

<p align="right">完成时间：2025年4月8日22时32分</p>

要点解析：

- 分情况处理，分别处理 所求值>sum，所求值= sum，没有所求值，已经存在所求值的情况
  1. 所求值 > sum，直接返回-1
  2. 所求值 = sum 直接返回 numsSize
  3. 通过 if(curr == sum)ans = MAX(ans,count);来更新ans值，如果最后ans值未进行变动即为0，则为没有所求值
  4. 存在所求值，我们的ans值指的是求得总和-所求值的长度，所以返回的长度应该是numsSize-ans