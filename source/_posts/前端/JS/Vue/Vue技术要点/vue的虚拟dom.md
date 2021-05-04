---
title: vue的虚拟dom
cover: false
categories:
  - 前端
  - JS
  - Vue
  - Vue 技术要点
tags:
  - vue
  - dom
  - 虚拟dom
  - diff
abbrlink: e11777d4
date: 2020-09-26 18:52:12
updated:
---
频繁地操作DOM非常地消耗性能，这主要是因为操作dom的过程涉及到js解析（JS引擎）和页面渲染（渲染引擎）的通信，作为独立的两个线程，这种通信是很耗性能的。

## 虚拟dom

通过状态生成一个虚拟节点树，然后使用虚拟节点树进行渲染，假如是首次节点的渲染就直接渲染，但是第二次的话就需要进行虚拟节点树的比较，只渲染不同的部分。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/virtual-dom.jpg" width=700 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">虚拟dom</div>
</center>

{% note info %}
Virtual DOM 本质上就是在 JS 和 DOM 之间做了一个缓存，它的优势不在于单次的操作，而是在大量、频繁的数据更新下，能够对视图进行合理、高效的更新。
{% endnote %}

在vue中，通过模板函数对html进行解析编译，从而转变成渲染函数，渲染函数执行后就变成了虚拟DOM节点树。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/vue%E6%A8%A1%E6%9D%BF%E8%A7%A3%E6%9E%90.png" width=700 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">模板解析</div>
</center>
vNode是一个普通的JS对象，保存了DOM节点需要的一些数据，比如文本节点，属性等，以DOM对象的形式表现出来。

当虚拟节点准备映射到视图的时候，为了避免额外的性能开销，会先和上一次的虚拟DOM节点树进行比较，然后只渲染不同的部分，然后更新到视图中，无需改动其他的节点状态，其中主要的技术就是节点比对算法patch。

考虑到性能问题，vue的状态侦测只能到某一个组件上面，组件内部通过diff算法来比对，从而渲染试图。

## diff算法
仅在同级的vnode间做diff，递归地进行同级vnode的diff，最终实现整个DOM树的更新，而非对树进行逐层搜索遍历的方式，且为跨层级的操作是非常少的，所以时间复杂度只有O(n)，是一种相当高效的算法。

算法的3个重要步骤：
1. 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
2. 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异
3. 把所记录的差异应用到所构建的真正的DOM树上，视图就更新了

### 实现过程
diff 算法本身非常复杂，实现难度很大。

几个比较的点如下：
- 判断Vnode和oldVnode是否指向同一个对象，如果是，那么直接return
- 如果他们都有文本节点并且不相等，那么将el的文本节点设置为Vnode的文本节点。
- 如果oldVnode有子节点而Vnode没有，则删除el的子节点
- 如果oldVnode没有子节点而Vnode有，则将Vnode的子节点真实化之后添加到el

