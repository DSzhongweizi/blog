---
title: Mrakdown——轻量级标记语言
date: 2018-01-05 00:00:00
---

{% tabs Mrakdown %}
<!-- tab 基础用法 -->
### 标题
- `# 一级标题`
- `## 二级标题`
- `### 三级标题`
- `#### 四级标题`
- `##### 五级标题`
- `###### 六级标题`

### 段落
段落的换行是使用
- 两个以上空格加上回车
- 空行

### 文本
`*`可以用`_`代替
- `*斜体*`
- `**粗体**`
- `***粗斜体***`

### 线
- 分割线：`---` 或 `___` 或 `***` （三个以上，）
- 删除线：`~~内容~~`
- 下划线：`<u>内容</u>`

### 列表
- 无序：`-` 或 `+` 或 `*`
- 有序：`数字.`

### 块
- 引用：`>`
- 代码块：三个反引号包裹 

### 链接
> 图片链接在名称前加`!`即可
- 普通：`[链接名称](链接地址)`
- 高级：`[链接名称][链接变量]` 和 `[链接变量]:链接地址` （不同行）

### 表格
```
表  头 | 表  头
 ----  |  ----  
单元格 | 单元格 
单元格 | 单元格 
```
对齐方式
```
| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
```
<!-- endtab -->

<!-- tab HTML元素 -->
不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写。

目前支持的 HTML 元素有：`<kbd>` `<b>` `<i>` `<em>` `<sup>` `<sub>` `<br>`等 ，如：

`kbd`用法：<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd>
<!-- endtab -->

<!-- tab 数学公式-->
`$$公式$$`
<!-- endtab -->
{% endtabs %}
