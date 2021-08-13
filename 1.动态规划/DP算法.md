#### [爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

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
    for(let i = 3;i <= n;i++){
        dp[i] = dp[i-2] + dp[i-1];
    }
    return dp[n];
};
```



# [打家劫舍](https://leetcode-cn.com/problems/house-robber/)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。

输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

```javascript
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



# [最大正方形](https://leetcode-cn.com/problems/maximal-square/)

在一个由 `'0'` 和 `'1'` 组成的二维矩阵内，找到只包含 `'1'` 的最大正方形，并返回其面积。![max1grid](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

```
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：4
```

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

# [零钱兑换](https://link.juejin.cn/?target=https%3A%2F%2Fleetcode-cn.com%2Fproblems%2Fcoin-change%2F)

给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。

计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。

你可以认为每种硬币的数量是无限的。

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

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



# [不同路径](https://leetcode-cn.com/problems/unique-paths/)

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

