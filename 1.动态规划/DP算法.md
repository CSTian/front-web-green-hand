**动态规划算法**

​		动态规划算法是通过拆分问题，定义问题状态和状态之间的关系，使得问题能够以递推（或者说分治）的方式去解决。
​		动态规划算法的基本思想与分治法类似，也是将待求解的问题分解为若干个子问题（阶段），按顺序求解子阶段，前一子问题的解，为后一子问题的求解提供了有用的信息。在求解任一子问题时，列出各种可能的局部解，通过决策保留那些有可能达到最优的局部解，丢弃其他局部解。依次解决各子问题，最后一个子问题就是初始问题的解。

​		基本步骤：找状态转移方程=>建立合适的数据结构表=>填表

## 1.[爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 *n* 是一个正整数。

```
示例1:
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶

示例2:
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

**解题思路：**

​		假设到达第n个台阶的方法数为F(n)，因为每次你可以爬 1 或 2 个台阶。所以你可以从第n-1个台阶或第n-2个台阶到达第n个台阶。

​		我们将其用数学公式表示即为` F(n) = F(n - 1) + F(n - 2)`

​		上述公式在`n >= 3`时成立。对于`0 <= n < 3 `存在`F(0) = 0,F(1) = 1,F(2) = 2`

```js
/**
 * @param {number} n
 * @return {number}
 */
//第0阶的方法为0种，第一阶的方法为1种，到达第二阶的方法为2种（爬两个1台阶or爬一个2台阶）
//对于任意的n>=3，s(n)=s(n-1) + s(n-2),因为到达n阶的最后一步只有两种可能，一种是走了一阶，一种是走了两阶。
var climbStairs = function(n) {
		let dp = [];
    dp[0] = 0,dp[1] = 1,dp[2] = 2;
    for(let i = 3; i <= n; i++){
        dp[i] = dp[i - 2] + dp[i - 1];
    }
    return dp[n];
};
```



## 2.[打家劫舍](https://leetcode-cn.com/problems/house-robber/)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```
示例1：
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。

示例2：
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

**解题思路：**

​		同样的思路我们假设第n个房间得到的金额为`F(n)`，因为相邻两个房间不能同时盗窃。

​		对于`F(n)`金额有两种可能，一种是上次偷得是`F(n - 1)`并没有偷`F(n)`，另一种则是上次偷了`F(n-2)`，然后这次偷了`F(n)`

​		即`F(n) = Max(F(n - 1),F(n - 2) + F(n))`

​		得到状态转移方程后即可遍历数组求得所需要的值

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
//第n个房间的值=Max（第n-1个房间总的值，第n-2个房间总的值+第n个值）
    if(nums.length === 0) return 0;
    if(nums.length === 1) return nums[0];
    if(nums.length === 2) return Math.max(nums[0],nums[1]);
    let dp = [nums[0],Math.max(nums[0],nums[1])];
    for(let i = 2; i < nums.length; i ++){
        dp[i] = Math.max(dp[i - 1],dp[i - 2] + nums[i]);
    }
    return dp[nums.length - 1];
};
```







## 3.[最大正方形](https://leetcode-cn.com/problems/maximal-square/)

在一个由 `'0'` 和 `'1'` 组成的二维矩阵内，找到只包含 `'1'` 的最大正方形，并返回其面积。![max2grid](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

```
示例1：
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：4

示例2：
输入：matrix = [["0","1"],["1","0"]]
输出：1

示例3：
输入：matrix = [["0"]]
输出：0
```

**解题思路**：

​		首先求最大面积即求最大边长。

​		设` dp[m][n]`用来存储这个点为右下角的最大正方形的边长

​		对`dp[m][n]`来说他是正方形的右下角，`dp[m-1][n],dp[m][n-1],dp[m-1][n-1]`为其相邻的三个点

​		对于`(i,j)`这个点来说

  1. 如果该位置的值是 0，则 `dp(i, j) = 0`，因为当前位置不可能在由 1 组成的正方形中；

  2. 如果该位置的值是 1，则 `dp(i, j)` 的值由其上方、左方和左上方的三个相邻位置的 `dp `值决定。具体而言，当前位置的元素值等于三个相邻位置的元素中的最小值加 1，状态转移方程如下：

     ​															`dp(i, j)=min(dp(i−1, j), dp(i−1, j−1), dp(i, j−1))+1`

```javascript
/**
 * @param {character[][]} matrix
 * @return {number}
 */
var maximalSquare = function(matrix) {
  	//求最大面积即求最大边长
  	//动态规划第一步，先假象我们创建了一个二维数组dp，用来存储「这个点为右下角的最大正方形的边长」
  	//将dp[m][n]看做正方形的右下角，dp[m-1][n],dp[m][n-1],dp[m-1][n-1]为其相邻的三个点
  	//dp[i][j] = Math.min(dp[i-1][j], dp[i-1][j-1], dp[i][j-1]) + 1
    let maxSqlen = 0;
    let rowLength = matrix.length, colLength = matrix[0].length;
    for(let row = 0; row < rowLength; row++){
        for(let col = 0; col < colLength; col++){
            if(matrix[row][col] === '1'){
                matrix[row][col] = Number(matrix[row][col]);
                if(row !==0 && col !== 0){
                    matrix[row][col] = Math.min(matrix[row - 1][col],matrix[row][col - 1],matrix[row - 1][col - 1]) + 1;
                }
                maxSqlen = Math.max(maxSqlen,matrix[row][col]);
            }
        }
    }
    return maxSqlen**2;
};
```

## 4.[零钱兑换](https://link.juejin.cn/?target=https%3A%2F%2Fleetcode-cn.com%2Fproblems%2Fcoin-change%2F)

给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。

计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。

你可以认为每种硬币的数量是无限的。

```
示例 1：
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1

示例 2：
输入：coins = [2], amount = 3
输出：-1

示例 3：
输入：coins = [1], amount = 0
输出：0

示例 4：
输入：coins = [1], amount = 1
输出：1

示例 5：
输入：coins = [1], amount = 2
输出：2

提示：
1 <= coins.length <= 12
1 <= coins[i] <= 231 - 1
0 <= amount <= 104
```

**解题思路：**

我们用`dp[i]`来存储当金额为`i`时所需最少的硬币个数

对于`dp[i]`来说到达`i`有`coins.length`种方式，即

`dp[i] = min(dp[i - coins[0],dp[i - coins[1],dp[i - coins[2],...,dp[i - coins[coins.length - 1]])`

```js
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */

var coinChange = function(coins, amount) {
    let dp = new Array( amount + 1 ).fill( Infinity );
    dp[0] = 0;
    for(let i = 1;i <= amount; i++){
        for(let coin of coins){
            if(i - coin >= 0){
                dp[i] = Math.min(dp[i], dp[i - coin] + 1)
            }
        }
    }
    return dp[amount] === Infinity ? -1 : dp[amount];
};
```



## 5.[不同路径](https://leetcode-cn.com/problems/unique-paths/)

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

```
示例1：
```

![robot_maze](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
输入：m = 3, n = 7
输出：2

示例2：
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下

示例 3：
输入：m = 7, n = 3
输出：28

示例 4：
输入：m = 3, n = 3
输出：6

提示：
1 <= m, n <= 100
题目数据保证答案小于等于 2 * 10^9
```

**解题思路：**

由于机器人只能向右或向下，所以对于任意的`(i,j) i!==0&&j!==0 `有`dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`

```js
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */

var uniquePaths = function(m, n) {
    const f = new Array(m).fill(0).map(() => new Array(n).fill(0));
    for(let i = 0;i < m; i++){
        f[i][0] = 1;
    }
    for(let i = 0;i < n; i++){
        f[0][i] = 1;
    }
    for(let i = 1; i < m; i++){
        for(let j = 1; j < n; j++){
            f[i][j] = f[i - 1][j] + f[i][j - 1];
        }
    }
    return f[m - 1][n - 1];
};
```

