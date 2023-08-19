### 2741.Special-Permutations

考虑到数据规模，算法基本是暴力搜索。我们确定第i位取数字p的时候，那么第i+1位的选择会是有限的几种（要求相邻两位互质）。于是就可以穷举这些选择，然后就用相同的形式递归处理下一个位置。此外，为了记录我们已经选择了哪些数字（不能用于当前的选择），我们需要一个n位的二进制编码state来记录。

由此，我们可以写出基本的DFS形式。令`dfs(i,p,state)`表示在第i个位置取数字p、并且已经选取的数字集合编码是state时，可以构造的合法序列的个数。我们有递归的框架：
```cpp
dfs(int i, int p, int state) {
   int ret = 0;
   for (int q: 与p互质且不在state里的数字) {
       ret += dfs(i+1, q, state+(1<<q));
   }
   return ret;
}
```

事实上，i可以从state里bit 1的个数推算出来，所以独立的参量其实就是p和state。因此我们可以开辟数组`memo[n][1<<n]`进行记忆化。

考虑到不是所有的状态`(p,state)`都合理，所以DFS会有很高的剪枝余地，效率更高。反之用DP的话，我们会盲目计算所有的`(p,state)`，会TLE。