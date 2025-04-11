# [不定滑动窗口（求最短/最小)](https://leetcode.cn/discuss/post/0viNMK/)

- [x] 209.长度最小的子数组
- [ ] 2904.最短且字典序最小的美丽子字符串
- [x] 1234.替换子串得到平衡字符串 1878
- [x] 2875.无限数组的最短子数组 1914
- [ ] 76.最小覆盖子串
- [ ] 632.最小区间 做法不止一种

##  [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

给定一个含有 `n` 个正整数的数组和一个正整数 `target` **。**

找出该数组中满足其总和大于等于 `target` 的长度最小的 **子数组** `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度**。**如果不存在符合条件的子数组，返回 `0` 。

```c
#define MIN(a,b) (a) < (b) ? (a) : (b)
int minSubArrayLen(int target, int* nums, int numsSize) {
    int curr = 0,ans = INT_MAX;
    int left = 0;
    for(int right = 0; right < numsSize; right++) {
        curr += nums[right];
        while(curr >= target) {
            ans = MIN(ans,right - left + 1);
            curr -= nums[left++];
        }
    }
    if(ans == INT_MAX) ans = 0;
    return ans;
}
```

要点解析：

- 让ans为int的最大值，保证后面随时可以更新，最后检查ans是否更新过，没更新过则说明没有这个子数组，置0
- 在while循环中更新左窗口和ans

<p align="right">完成时间：2025年4月9日09点27分</p>

## [2904. 最短且字典序最小的美丽子字符串](https://leetcode.cn/problems/shortest-and-lexicographically-smallest-beautiful-string/)

给你一个二进制字符串 `s` 和一个正整数 `k` 。

如果 `s` 的某个子字符串中 `1` 的个数恰好等于 `k` ，则称这个子字符串是一个 **美丽子字符串** 。

令 `len` 等于 **最短** 美丽子字符串的长度。

返回长度等于 `len` 且字典序 **最小** 的美丽子字符串。如果 `s` 中不含美丽子字符串，则返回一个 **空** 字符串。

对于相同长度的两个字符串 `a` 和 `b` ，如果在 `a` 和 `b` 出现不同的第一个位置上，`a` 中该位置上的字符严格大于 `b` 中的对应字符，则认为字符串 `a` 字典序 **大于** 字符串 `b` 。

- 例如，`"abcd"` 的字典序大于 `"abcc"` ，因为两个字符串出现不同的第一个位置对应第四个字符，而 `d` 大于 `c` 。

```c

```

> 返回数组的暂时做不到，先跳过

## [1234. 替换子串得到平衡字符串](https://leetcode.cn/problems/replace-the-substring-for-balanced-string/)

有一个只含有 `'Q', 'W', 'E', 'R'` 四种字符，且长度为 `n` 的字符串。

假如在该字符串中，这四个字符都恰好出现 `n/4` 次，那么它就是一个「平衡字符串」。

 

给你一个这样的字符串 `s`，请通过「替换一个子串」的方式，使原字符串 `s` 变成一个「平衡字符串」。

你可以用和「待替换子串」长度相同的 **任何** 其他字符串来完成替换。

请返回待替换子串的最小可能长度。

如果原字符串自身就是一个平衡字符串，则返回 `0`。

 ```c
#define MIN(a,b) (a) > (b) ? (b) : (a)
int balancedString(char* s) {
    int ans = INT_MAX, left = 0;
    int len = strlen(s);
    int blen = len / 4;
    int Qnum = 0, Wnum = 0, Enum = 0, Rnum = 0;
    for(int i = 0; i < len; i++) {
        if(s[i] == 'Q')Qnum++;
        if(s[i] == 'W')Wnum++;
        if(s[i] == 'E')Enum++;
        if(s[i] == 'R')Rnum++;
    }
    if(Qnum <= blen && Wnum <= blen && Enum <=blen & Rnum <= blen) return 0;
    for(int right = 0; right < len; right++) {
        // 窗口外面的数量
        if(s[right] == 'Q')Qnum--;
        if(s[right] == 'W')Wnum--;
        if(s[right] == 'E')Enum--;
        if(s[right] == 'R')Rnum--;
        // 如果窗口外面的字符平衡，则记录长度，并把左指针右移
    while(left <= right &&Qnum <= blen && Wnum <= blen && Enum <=blen && Rnum <= blen){
        ans = MIN(right - left + 1,ans);
        if(s[left] == 'Q')Qnum++;
        if(s[left] == 'W')Wnum++;
        if(s[left] == 'E')Enum++;
        if(s[left] == 'R')Rnum++;
        left++;
    }

    }
    return ans;
}
 ```

要点解析：

- 将问题转换为减去窗口内的字符串后，窗口外的字符串是否平衡，平衡则记录下来，并移动左指针，如果移动完左指针不平衡了，就移动右指针，如果移动完左指针依旧平衡，则继续移动左指针。

<p align="right">完成时间：2025年4月10日09点21分</p>	

## [2875. 无限数组的最短子数组](https://leetcode.cn/problems/minimum-size-subarray-in-infinite-array/)

给你一个下标从 **0** 开始的数组 `nums` 和一个整数 `target` 。

下标从 **0** 开始的数组 `infinite_nums` 是通过无限地将 nums 的元素追加到自己之后生成的。

请你从 `infinite_nums` 中找出满足 **元素和** 等于 `target` 的 **最短** 子数组，并返回该子数组的长度。如果不存在满足条件的子数组，返回 `-1` 。

```c
#define MIN(a,b) (a) < (b) ? (a) : (b)
int minSizeSubarray(int* nums, int numsSize, int target) {
    int left = 0,  ans = INT_MAX;
    long long sum = 0, curr = 0;
    for(int i = 0; i < numsSize; i++) {
        sum += nums[i];
    }
    int term = target / sum;
    int remain = target % sum;
    if(remain == 0) return term*numsSize;
    for(int right = 0; right < 2 * numsSize ; right++) {
        curr += nums[right % numsSize];
        while(curr > remain && left <= right) {
            curr -= nums[left % numsSize];
            left++;
        }
        if(curr == remain)ans = MIN(ans,right - left + 1);
    }
    if(ans == INT_MAX) return -1;
    ans += term*numsSize;
    return ans;
}
```

要点解析：

- 将问题转换一下，<font color = "red">将原本要求的target转换为target%sum</font>，这样子的话，我们只需要两倍长度的循环数组，求得解出这个值的最小值，再加上numsSize的倍数即可，因为这一部分是绝对需要整个的数组来组成

<p align="right">完成时间：2025年4月10日16点07分</p>	

## [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

```c

```

