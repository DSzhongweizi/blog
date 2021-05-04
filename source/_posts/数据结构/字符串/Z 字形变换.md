---
title: Z 字形变换
categories:
  - 数据结构
  - 字符串
tags:
  - 字符串
cover: false
abbrlink: a5661a6f
date: 2020-10-19 20:19:33
---
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

    L   C   I   R
    E T O E S I I G
    E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数： `string convert(string s, int numRows);`

代码：
```js
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */
var convert = function(s, numRows) {
    let res = [], rowMax=Math.min(numRows,s.length);    //确定行数，不浪费多余空间
    
    let up = false; //表示向上
    for(let i=0,row=0;i<s.length;i++){
        if(i < rowMax) res.push([]);
        res[row].push(s.charAt(i));
        up ? row-- : row ++;
        if(row == rowMax-1 || row == 0) up = !up;   //触碰到上边界/下边界就反向
    }
    return res.flat().join('');
};
```