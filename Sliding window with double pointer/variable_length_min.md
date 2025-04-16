# [不定滑动窗口（求最短/最小)](https://leetcode.cn/discuss/post/0viNMK/)

- [x] [209.长度最小的子数组](#209-长度最小的子数组)
- [x] [2904.最短且字典序最小的美丽子字符串](#2904-最短且字典序最小的美丽子字符串)
- [x] [1234.替换子串得到平衡字符串 1878](#1234-替换子串得到平衡字符串)
- [x] [2875.无限数组的最短子数组 1914](#2875-无限数组的最短子数组)
- [x] [76.最小覆盖子串](#76-最小覆盖子串)
- [x] [632.最小区间 做法不止一种](#632-最小区间)

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
char* shortestBeautifulSubstring(char* s, int k) {
    // 先判断是否存在子数组
    int sum = 0;
    for(int i = 0; s[i]; i++) {
        if(s[i] == '1') sum++;
    }
    if(sum < k) return "";

    // 若存在子数组
    int left = 0, ans_left = -1, ans_right = INT_MAX-1;
    int curr = 0;
    for(int right = 0; s[right]; right++){  
        if(s[right] == '1')curr++;
        while(curr == k) {
            if(right - left < ans_right - ans_left){
                ans_left = left;
                ans_right = right;
            }
            // 长度相同比较字典序
            if(right - left == ans_right - ans_left){
                if(strncmp(s+left,s+ans_left,right - left +1) <0) {
                    ans_left = left;
                    ans_right = right;
                }
            }
            if(s[left] == '1')curr--;
            left++;
        }
  
    }
    s[ans_right + 1] = '\0';
    return s+ans_left;
} 
```

要点解析：

- 返回子串还是那一套

> `s[ans_right + 1] = '\0';`
> `return s+ans_left;`

- 比较字典序可以用strncmp(a,b,len)；,如下，因为传入s的时候，数组退化为首指针，因此用left作为偏移量，其中最后的right-left+1则作为长度，比较这么长即可

> `strncmp(s+left,s+ans_left,right - left +1)`

- 维护窗口还是那老一套

<p align="right">完成时间：2025年4月12日10点12分</p>



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
char* minWindow(char* s, char* t) {
    int ans_right = INT_MAX /2, ans_left = -1;
    int cnt[128] = {};
    int kind = 0;
    for(int i = 0; t[i]; i++) {
        if(cnt[t[i]]++ == 0) kind++;
    }
    int left = 0;
    for(int right = 0; s[right]; right++) {
        if(--cnt[s[right]] == 0)kind--;
        while(kind == 0) {
            if(right - left < ans_right - ans_left){
                ans_left = left;
                ans_right = right;
            }
            if(cnt[s[left++]]++==0)kind++;
        }
    }
    if(ans_left == -1) return"";
    s[ans_right + 1] = '\0';
    return s+ans_left;

}
```

要点解析：

- 对于返回子串应该这样操作：
- <font color = "Red">①找到左右指针②将右指针+1的元素置为'\0'（原地返回）③返回ans+left（而非ans[left]），因为返回的时候数组退化成了指针的首地址。</font>

>本题中如下：
>
>`s[ans_right + 1] = '\0';`
>`return s+ans_left;`

- 老技巧判断当前窗口有几个相同元素，用128的cnt数组映射，然后进入窗口时候cnt为0则种类加1，这是遍历子串做的
- 当第二次遍历主数组的时候，进入窗口时--cnt[]==0，则说明这个元素是子串里有的并且进入了窗口，如果出去窗口时，未经操作的时候他为0，说明这个元素是子串里有的，并且出去后窗口没有这个元素了，所以得对kind++,然后左指针右移。

<p align="right">完成时间：2025年4月12日9点08分</p>	

## [632. 最小区间](https://leetcode.cn/problems/smallest-range-covering-elements-from-k-lists/)

你有 `k` 个 **非递减排列** 的整数列表。找到一个 **最小** 区间，使得 `k` 个列表中的每个列表至少有一个数包含在其中。

我们定义如果 `b-a < d-c` 或者在 `b-a == d-c` 时 `a < c`，则区间 `[a,b]` 比 `[c,d]` 小。

```c
 typedef struct {
    int value;
    int key;
 }Node;
 int compare(const void* a, const void* b) {
    Node *num1 = (Node*)a;
    Node *num2 = (Node*)b;
    return num1->value - num2->value;
 }
int* smallestRange(int** nums, int numsSize, int* numsColSize, int* returnSize) {
        int len = 0;
        for (int i = 0; i < numsSize; i++) {
        len += numsColSize[i];
    }
        Node *arr =(Node*)malloc(sizeof(Node)* len);
        int idx = 0;
    for(int i = 0; i < numsSize; i++) {
        for(int j = 0; j < numsColSize[i]; j++) {
            arr[idx].value = nums[i][j];
            arr[idx++].key = i;
        }
    }
    qsort(arr, len, sizeof(Node),compare);
    int left = 0,ans = INT_MAX;
    int count = 0; // 种类
    int *kind = (int*)calloc(numsSize, sizeof(int));
    int *ans_arr =(int *)malloc(sizeof(int)*2);
    for(int right = 0; right < len; right++) {
        if(kind[arr[right].key]++ == 0)count++;
        while(count == numsSize) {
            if((arr[right].value - arr[left].value) < ans){
                ans = arr[right].value - arr[left].value;
                ans_arr[0] = arr[left].value;
                ans_arr[1] = arr[right].value;
            } 
            if (--kind[arr[left].key] == 0) count--;
            left++;        
        }
    }
    *returnSize =  2;
    return ans_arr;
} 
```

要点解析：

- 开拓思路，`将题目的二维数组转化为一维数组在在根据值进行排序`，这个时候就可以使用滑动窗口进行解题

- 老方法，对于种类的一个统计还是要善于使用映射

> `if(kind[arr[right].key]++==0)count++;`
>
> `if (--kind[arr[left].key] == 0) count--;`

- j < numsColSize[i]而非j< numsColSize

  <p align="right">完成时间：2025年4月11日16点41分</p>	