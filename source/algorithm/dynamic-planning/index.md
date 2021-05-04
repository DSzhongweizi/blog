---
title: 动态规划
date: 2018-06-07 22:17:49
type: "dynamic-planning"
---
参考
- [希望用一种规律搞定背包问题](https://leetcode-cn.com/problems/combination-sum-iv/solution/xi-wang-yong-yi-chong-gui-lu-gao-ding-bei-bao-wen-/)


自顶向下，记忆化搜索，递归求解：
由于有大量「重复子问题」，因此必须使用缓存，以避免相同问题重复求解，这个方法叫「记忆化搜索」，自上而下，直接面对问题求解，遇到什么问题，就解决什么问题，同时记住结果；

自底向上，表格记住子问题解，可以不直接面对问题求解，从这个问题最小的样子开始，通过逐步递推，至到得到所求的问题的答案。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/dp.png" />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">动态规划问题思考</div>
</center>