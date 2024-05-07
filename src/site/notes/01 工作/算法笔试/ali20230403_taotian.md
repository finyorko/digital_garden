---
{"type":"笔试","title":"[[ali20230403_taotian]]","author":"fun","tags":["笔试","算法"],"date":"2023-04-03","comments":true,"publish":true,"dg-publish":true,"permalink":"/01 工作/算法笔试/ali20230403_taotian/","dgPassFrontmatter":true,"created":"2024-04-22T22:05:21.548+08:00","updated":"2024-04-22T22:23:45.995+08:00"}
---




## 题目
### 第一题

签到题


### 第二题

小红在玩战略游戏，地图上有n个国家排成一排，小红的军队初始时在1号国家，她需要率领军队进攻n号国家。
军队每从一个国家移动到一个相邻国家就需要消耗1单位的粮食，每个国家都会售卖粮食，每单位价格为$a_i$，小红可以购买任意单位的粮食。
小红想要用最少的花费使得军队到达n号国家，但是她希望一个国家的粮食不要购买太多，即重复吃一个国家粮食的次数的最大值最小。
小红想知道一种她要到达n号国家且满足上述条件的粮食购买方案。（如果有多种答案，可以输出任意一种)

**输入描述：**

第一行输入一个整数n，表示国家个数  
第二行输入n个整数，表示每个国家的粮食售价

**输出描述：**

输出一行整数表示答案  

**示例：**

**输入：**

4
1 2 1 4 

**输出：**

2 0 1 0

**说明：**

在第1一个国家购买2单位粮食，花费2  
在第2一个国家购买0单位粮食，花费0  
在第3一个国家购买1单位粮食，花费1  
在第4一个国家购买0单位粮食，花费0  
此时花费为3，吃同一种粮食次数最多的1号国家的粮食，为2次。

**思路：**（#TODO）


**代码实现：**
```cpp
#include<iostream>
#include<bits/stdc++.h>

using namespace std;

// 单调栈
// 时间复杂度：O(n)
// 空间复杂度：O(n)
// vector<int> get_plan(vector<int> nums){
//     int n = nums.size();
//     vector<int> ans(n, 1);
//     ans[n - 1] = 0;
//     stack<int> st;
//     st.push(n - 1);
//     for(int i = n - 2; i >= 0; i--){
//         int x = nums[i];
//         while(!st.empty() && x < nums[st.top()]){
//             ans[i] += ans[st.top()];
//             ans[st.top()] = 0;
//             st.pop();
//         }
//         st.push(i);
//     }
//     return ans;
// }

// 扫描一遍，记录前缀最小值
// 时间复杂度：O(n)
// 空间复杂度：O(n)
vector<int> get_plan(vector<int> nums){
    int n = nums.size();
    vector<int> pre(n);
    // int temp_min = 0;
    vector<int> ans(n, 1);
    ans[n - 1] = 0;
    for(int i = 1; i < n; i++){
        if(nums[i] <= nums[pre[i - 1]]){
            pre[i] = i;
        }else{
            pre[i] = pre[i - 1];
            ans[pre[i]] += 1;
            ans[i] = 0;
        }
    }
    return ans;
}

int main(){
    int n;
    cin >> n;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin>>a[i];
    vector<int> ans = get_plan(a);
    for(int i : ans) cout<<i<<" ";
    cout<< " "<<endl;
    return 0;
}

```
### 第三题

小明有一个长为n的数组a，小红有一个长度为m的数组b。
小明希望对a中的元素进行一些变换，使得a的所有元素都在b中出现过。具体的，他可以做任意如下操作：  
选择i，另$a_i= a_i + 1$或者$a_i = a_i - 1$
小明想在变化后，同时又能使a单调下降，他想知道至少需要操作多少次。

**输入描述：****

第一行两个正整数n,m。分别表示a和b的长度。  
第二行n个正整数$a_i$，表示a数组初始时所有的元素。  
第三行m个正整数$b_i$，表示b数组的所有元素。

**输出描述：**

输出包含一行一个整数，表示最少操作次数。

**示例：**

**输入：**

5 4  
3 5 3 8 10  
1 2 3 4

**输出：**  
12

思路：动态规划，转移方程见代码

代码实现：

```cpp
#include <bits/stdc++.h>
#include <iostream>
using namespace std;

int min_operations(vector<int> a, vector<int> b)
{
    int n = a.size();
    int m = b.size();
    int ans;
    // 递归
    // vector<vector<int>> memo(n, vector<int>(m, -1));
    // function<int(int, int)> dfs = [&](int i, int j) -> int {
    //     if(i == 0) return abs(a[i] - b[j]);
    //     if(j == 0) return INT_MAX;
    //     auto &res = memo[i][j];
    //     if(res != -1) return res;
    //     return res = min(dfs(i - 1, j) + abs(a[i] - b[j]), dfs(i, j - 1));
    // };
    // ans = dfs(n - 1, m - 1);

    // 翻译成递推
    // int f[n + 1][m + 1];
    // memset(f, 0x3f, sizeof(f));
    // for(int j = 0; j <= m; j++){
    //     f[0][j] = 0;
    // }
    // for(int i = 0; i < n; i++){
    //     for(int j = 0; j < m; j++){
    //         f[i + 1][j + 1] = min(f[i][j + 1] + abs(a[i] - b[j]), f[i + 1][j]);
    //     }
    // }
    // ans = f[n][m];

    // 空间优化
    vector<int> f(m + 1);
    f[0] = INT_MAX;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            f[j + 1] = min(f[j + 1] + abs(a[i] - b[j]), f[j]);
        }
    }
    ans = f[m];
    return ans;
}

int main()
{
    // 记录
    int n, m;
    cin >> n >> m;
    vector<int> a(n), b(m);
    for (int i = 0; i < n; i++) cin >> a[i];
    for (int i = 0; i < m; i++) cin >> b[i];
    sort(b.begin(), b.end());
    int ans = min_operations(a, b);
    cout << ans << endl;
    return 0;
}
```
