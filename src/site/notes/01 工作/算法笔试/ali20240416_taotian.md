---
{"dg-publish":true,"permalink":"/01//ali20240416-taotian/","title":"[[ali20240416_taotian]]","tags":["笔试","算法"]}
---




## 题目
### 第一题

小红拿到了一个01串，她每次操作可以交换两个相邻字符。小红希望最终所有的"1"字符都连通，请你帮小红算出最少的操作次数

**输入描述**

输入一个长度不超过200000的01串。

  
**输出描述**

一个整数，代表小红的最少操作次数。

**示例：**


**输入：**
10101

**输出：**
2

**思路：** 找中位数，记录第i个1应该移动到的下标，用原来对应位置的下标相减

**代码实现：**

```python
def min_moves_to_group_ones(binary_string):
    # 找到所有'1'的索引
    ones_positions = [i for i, char in enumerate(binary_string) if char == '1']
    
    if not ones_positions:
        return 0
    
    # 计算中位数位置
    n = len(ones_positions)
    median_index = ones_positions[n // 2]
    
    # 计算最少操作次数
    min_moves = 0
    # 我们将'1'移动到median_index - (n//2) 到 median_index + (n//2)的位置
    target_start = median_index - (n // 2)
    for i, pos in enumerate(ones_positions):
        target_pos = target_start + i
        min_moves += abs(pos - target_pos)
    
    return min_moves

while True:
    binary_string = input("请输入一个01串：")
    print(min_moves_to_group_ones(binary_string))
```
### 第二题

小红拿到了一个数组，她进行恰好2次操作：选择任意一个元素x，使其乘以2

小红想知道，操作恰好2次后，数组的众数出现的次数最多是多少?

众数：是一组数据中出现次数最多的数值。

**输入描述：**

第一行输入一个正整数n，代表数组的元素数量。1≤n≤1e5

第二行输入n个正整数ai，代表数组的元素。1≤ai≤1e9

**输出描述：**

一个整数，操作恰好2次后数组的众数出现的次数。

**示例：**

**输入：**

3  
2 6 3

**输出：**

2


**示例：** 对第一个和第三个操作一次，变成[4,6,6]，众数出现了两次


**思路：**遍历数字a，用map计数，key为ai，value为ai出现的次数。
然后遍历map，假设当前key为num，找num/2和num/4出现的个数，num/2最多取两次，num/4最多取一次。（注意一定判断严格num/2或者num/4）

### 第三题

小红拿到了一棵树，树上有n个节点， n—1 条边。其中有一些边不够结实，小红可以把这条边砍掉。
小红希望最终砍出的每个子树大小不超过2。小红想知道自己最少砍多少刀可以达成目的？

**输入描述**

第一行输入一个正整数n，代表树的节点数量。  
接下来的 n-1 行，每行输入两个正整数u，v和一个字符c，代表节点u和节点v有一条边连接。字符C为'Y'代表这条边很结实，小红砍不了；为'N'代表不结实，可以砍。

1≤n≤1e5  
1≤u,v≤n

**输出描述**

如果小红无法达成目的，则输出-1。  
否则输出一个整数，代表小红砍的最少次数。


**示例1：**  

**输入：**  

4  
1 2 N  
2 3 N  
3 4 N

**输出：1**

**说明：只需要砍掉 2-3 即可**


**示例2**：

**输入**：  

4   
1 2 N  
2 3 Y  
3 4 N  

**输出：2**

**说明：只需要砍掉 1-2 和 2-3 这两条边**

**思路：**打家劫舍变种