# [定长滑动窗口(基础题)](https://leetcode.cn/discuss/post/0viNMK/)

- [x] [1456.定长子串中元音的最大数目 1263](#1456-定长子串中元音的最大数目---力扣leetcode)
- [x] [643.子数组最大平均数 I](#643-子数组最大平均数-i---力扣leetcode)
- [x] [1343.大小为 K 且平均值大于等于阈值的子数组数目 1317](#1343-大小为-k-且平均值大于等于阈值的子数组数目)
- [x] [2090.半径为 k 的子数组平均值 1358](#2090-半径为-k-的子数组平均值)
- [x] [2379.得到 K 个黑块的最少涂色次数 1360](#2379-得到-k-个黑块的最少涂色次数)
- [x] [2841.几乎唯一子数组的最大和 1546](#2841-几乎唯一子数组的最大和)
- [x] [2461.长度为 K 子数组中的最大和 1553](#2461-长度为-k-子数组中的最大和)
- [x] [1423.可获得的最大点数 1574](#1423-可获得的最大点数)
- [x] [1052.爱生气的书店老板](#1052-爱生气的书店老板)
- [x] [1652.拆炸弹 做到 O(n)](#1652-拆炸弹)

## [1456. 定长子串中元音的最大数目 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-number-of-vowels-in-a-substring-of-given-length/description/)

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



## [2379. 得到 K 个黑块的最少涂色次数](https://leetcode.cn/problems/minimum-recolors-to-get-k-consecutive-black-blocks/)

给你一个长度为 `n` 下标从 **0** 开始的字符串 `blocks` ，`blocks[i]` 要么是 `'W'` 要么是 `'B'` ，表示第 `i` 块的颜色。字符 `'W'` 和 `'B'` 分别表示白色和黑色。

给你一个整数 `k` ，表示想要 **连续** 黑色块的数目。

每一次操作中，你可以选择一个白色块将它 **涂成** 黑色块。

请你返回至少出现 **一次** 连续 `k` 个黑色块的 **最少** 操作次数。

```c
int minimumRecolors(char* blocks, int k) {
    int len= strlen(blocks);
    int now_window=0,max_window=0;            // 当前窗口的黑块数量和记录窗口的最大黑块数量
    for( int right=0; right < len; right++) {
        if(blocks[right] == 'B') now_window++;    // 右指针指向黑方块，则now_window++
        if(right< k - 1) continue;
        if(max_window < now_window) max_window = now_window;  // 更新最大值
        int left =right - k + 1;
        if(blocks[left] == 'B') now_window--;     // 处理出去的窗口
    }
    return k-max_window;
}
```

要点解析：

- 本题需要将思路转换，把题目转化成<font color ="red">统计每个窗口中的黑块数量</font>，我们可以利用滑动窗口找到窗口中拥有的最大黑块数量

<p align="right">完成时间：2025年3月13日15点17分</p>



## [2841. 几乎唯一子数组的最大和](https://leetcode.cn/problems/maximum-sum-of-almost-unique-subarray/)

给你一个整数数组 `nums` 和两个正整数 `m` 和 `k` 。

请你返回 `nums` 中长度为 `k` 的 **几乎唯一** 子数组的 **最大和** ，如果不存在几乎唯一子数组，请你返回 `0` 。

如果 `nums` 的一个子数组有至少 `m` 个互不相同的元素，我们称它是 **几乎唯一** 子数组。

子数组指的是一个数组中一段连续 **非空** 的元素序列。

> 本题要用到哈希表，因此花费了较多时间去复习哈希表的数据结构

```c
#define max_size 20000
#define max(a, b) ((a) > (b) ? (a) : (b))
typedef struct {
    int value[max_size];                   // 数组中的关键字
    int key[max_size];                     // 数组中关键字出的次数
    int size;                              // 哈希表不同元素的个数
}Hashmap;
// 初始化哈希表
void init_Hashmap(Hashmap* map) {  
    map->size=0;                             // 哈希表目前零个元素
    memset(map->key,0,sizeof(map->key));     // 初始化
    memset(map->value,0,sizeof(map->value)); // 初始化
}
// 插入操作
void insert_Hashmap (Hashmap* map, int val) {
    // for循环检查是否有相同元素
    for(int i = 0; i <map->size; i++){
        if(map->key[i] == val) {
            map->value[i]++;                //计数器+1
            return;
        }
     }
    // 没有相同元素的操作
    map->key[map->size] = val;             // 加入该关键字
    map->value[map->size] = 1;             // 计算器+1
    map->size++;                           // 哈希表有的元素+1
   
}
void remove_Hashmap(Hashmap* map,int val) {
    // for循环找到去除的元素进行—1操作
    for(int i = 0; i<map->size; i++){
        if(map->key[i] == val){
            map->value[i]--;
            // 如若溢出的元素仅有一个，那么将最右边的函数放到这个位置上，使得哈希表紧凑
            if(map->value[i] == 0){
                map->key[i] = map->key[map->size-1];
                map->value[i]=map->value[map->size-1];
                map->size--;
            }
            return;
        }
    }
}
// 检查哈希表中有多少不同元素
int different_numbers(Hashmap* map, int m) { 
    return map->size >= m;        // 返回判断后的结果
    }

long long maxSum(int* nums, int numsSize, int m, int k) {
    long long ans = 0, max = 0;
    Hashmap map;
    init_Hashmap(&map);
    // 滑动窗口固定操作
    for(int right = 0; right < numsSize; right++) {
        insert_Hashmap(&map,nums[right]);
        max += nums[right];
        if(right < k-1) continue;
        if(different_numbers(&map, m)) ans = max(ans, max);
    long long left =nums[right - k + 1];
    remove_Hashmap(&map, left);
    max -= nums[right - k + 1];
    }
    return ans;
}
```

要点解析：

- 请仔细查看代码注释以复习哈希表的使用，然而这个专题是滑动窗口专题，还请关注滑动窗口的固定操作，学会套公式

<p align="right">完成时间：2025年3月14日15点53分</p>

## [2461. 长度为 K 子数组中的最大和](https://leetcode.cn/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

给你一个整数数组 `nums` 和一个整数 `k` 。请你从 `nums` 中满足下述条件的全部子数组中找出最大子数组和：

- 子数组的长度是 `k`，且
- 子数组中的所有元素 **各不相同 。**

返回满足题面要求的最大子数组和。如果不存在子数组满足这些条件，返回 `0` 。

**子数组** 是数组中一段连续非空的元素序列。

```c
#define N 100001
#define max(a,b) a>b?a:b
long long maximumSubarraySum(int* nums, int numsSize, int k) {
    long long ans = 0,sum = 0;
    int hash[N] = { 0 };
    int cnt = 0;
    for(int right=0; right<numsSize; right++){
        sum += nums[right];
        if(++hash[nums[right]] == 2){
            cnt++;
        }
        if(right < k - 1) continue;
        if(cnt == 0) {
            ans = max(ans,sum);
        }
        int left = right - k + 1;
        if(--hash[nums[left]] == 1) cnt--;
        sum -= nums[left];
    }
    return ans;
}
```

要点解析：

- 本题也可以参考上一题的哈希表处理方法，只不过会超时，想要解决超时也可以<font color="red">更换更高效的寻址方法</font>
- 本题使用关键字作为哈希表当中的数组index，而数组中存放的则是他的一个重复次数，如下，如若有重复关键字，那么就会有一个cnt全局变量告诉你，这个窗口中存在重复元素，如果哈希表中对应关键字唯一了，那么就说明窗口重复元素数量减一

> if(++hash[nums[right]] == 2){
>            cnt++;
>        }
>
>   if(--hash[nums[left]] == 1) cnt--;
>        sum -= nums[left];
>    }

- 其余的则是套滑动窗口的老公式

<p align="right">完成时间：2025年3月14日19点05分</p>

## [1423. 可获得的最大点数](https://leetcode.cn/problems/maximum-points-you-can-obtain-from-cards/)

几张卡牌 **排成一行**，每张卡牌都有一个对应的点数。点数由整数数组 `cardPoints` 给出。

每次行动，你可以从行的开头或者末尾拿一张卡牌，最终你必须正好拿 `k` 张卡牌。

你的点数就是你拿到手中的所有卡牌的点数之和。

给你一个整数数组 `cardPoints` 和整数 `k`，请你返回可以获得的最大点数。

```C
#define min(a,b) a>b?b:a 
int maxScore(int* cardPoints, int cardPointsSize, int k) {
    int windows = 0, right = 0, sum = 0, ans = INT_MAX;
    if(k == cardPointsSize){
        for(right = 0; right < cardPointsSize; right++){
            sum += cardPoints[right];
        }
        return sum;
    }
    for(right = 0; right < cardPointsSize; right++){
        windows += cardPoints[right];
        sum += cardPoints[right];
        if(right < cardPointsSize - k -1) continue;
        ans = min(ans,windows);
        int left = right -(cardPointsSize - k -1 );
        windows -= cardPoints[left];
    }
    return sum-ans;
}
```

要点解析：

- 逆向思维：<font color="red">从两边开始取值，说明中间则是连续的值，则可以利用滑动窗口找到最小值，</font>拿sum一减既是所要的最大值

- 套公式

<p align="right">完成时间：2025年3月14日22点37分</p>

## [1052. 爱生气的书店老板](https://leetcode.cn/problems/grumpy-bookstore-owner/)

有一个书店老板，他的书店开了 `n` 分钟。每分钟都有一些顾客进入这家商店。给定一个长度为 `n` 的整数数组 `customers` ，其中 `customers[i]` 是在第 `i` 分钟开始时进入商店的顾客数量，所有这些顾客在第 `i` 分钟结束后离开。

在某些分钟内，书店老板会生气。 如果书店老板在第 `i` 分钟生气，那么 `grumpy[i] = 1`，否则 `grumpy[i] = 0`。

当书店老板生气时，那一分钟的顾客就会不满意，若老板不生气则顾客是满意的。

书店老板知道一个秘密技巧，能抑制自己的情绪，可以让自己连续 `minutes` 分钟不生气，但却只能使用一次。

请你返回 *这一天营业下来，最多有多少客户能够感到满意* 。

```c
#define max(a, b) a > b ? a : b
int maxSatisfied(int* customers, int customersSize, int* grumpy, int grumpySize, int minutes) {
    int ans = 0, win = 0,sum_not_angry=0;
    for(int right = 0; right<customersSize; right++) {
        if(grumpy[right] == 0) sum_not_angry += customers[right];          // 满意顾客总共人数
        if(grumpy[right] == 1)win += customers[right];                     // 老板生气时候的窗口人数
        if(right < minutes - 1 ) continue;
        ans = max(ans, win);                                               // 更新窗口最大值
        int left = right - minutes + 1;
        if(grumpy[left] == 1) win -=customers[left];                      // 老板生气时候的窗口人数离开
    }
    return sum_not_angry+ans;                                             // 窗口最大值+满意人数                      
}
```

要点解析：

- 转换问题，<font color="red">要找到老板不生气时候的客人数量，那么其实就是算出总共有多少人会满意（即老板步生气的时候有多少人），然后再用滑动数组算出老板生气的时候什么时候人最多，然后两者相加即是答案。</font>

- 还是老样子，滑动窗口套公式，只不过要配合if条件套

<p align="right">完成时间：2025年3月15日09点21分</p>

## [1652. 拆炸弹](https://leetcode.cn/problems/defuse-the-bomb/)

你有一个炸弹需要拆除，时间紧迫！你的情报员会给你一个长度为 `n` 的 **循环** 数组 `code` 以及一个密钥 `k` 。

为了获得正确的密码，你需要替换掉每一个数字。所有数字会 **同时** 被替换。

- 如果 `k > 0` ，将第 `i` 个数字用 **接下来** `k` 个数字之和替换。
- 如果 `k < 0` ，将第 `i` 个数字用 **之前** `k` 个数字之和替换。
- 如果 `k == 0` ，将第 `i` 个数字用 `0` 替换。

由于 `code` 是循环的， `code[n-1]` 下一个元素是 `code[0]` ，且 `code[0]` 前一个元素是 `code[n-1]` 。

给你 **循环** 数组 `code` 和整数密钥 `k` ，请你返回解密后的结果来拆除炸弹！

```c
int reduce(int* start, int* end) {                     // 加和函数
    int sum = 0;
    while (start != end) {
        sum += *start;
        start++;
    }
    return sum;
}
// 解密函数
int* decrypt(int* code, int codeSize, int k, int* returnSize) {
    int n = codeSize;
    int* ans = (int*)malloc(n * sizeof(int));   
    *returnSize = n;

    int right = k > 0 ? k + 1 : n;                               // 第一个窗口的右开端点
    k = abs(k);                                                  // k小于0的话绝对值处理
    int sum= reduce(code + right - k, code + right);
    for(int i=0; i < n; i++) {
        ans[i] = sum;
        int right_windows = right % n;                            // 随着右指针移动的入口
        int left_windosws = (right - k)%n;                        // 随着右指针移动的出口
        sum +=code[right_windows] - code[left_windosws];          // 更新
        right++;                                                 // 右移
    }

    return ans;
}

```

要点解析：

- <font color ="Red">对于k的处理，注意无论 *k*>0 还是 *k*<0，窗口都在向右移动，只有初始位置不同。</font>

 (a)当k>0时，直接对<font color="red">加一然后对数组长度取余</font>即可得到右窗口

​          k>0，第一个窗口的的下标范围为 [1,k+1)。

 (b)当k<=0时，令右指针为数组长度

​          k<0，第一个窗口的的下标范围为 [n−∣k∣,n)

- 滑动：在窗口向右滑动时，设移入窗口的元素下标为 r *mod* n*，则移出窗口的元素下标为 (*r*−∣*k*∣) mod *n*

<p align="right">完成时间：2025年3月15日11点42分</p>