---
title: MVC、MVP和MVVM
cover: false
tags:
  - mvc
  - mvp
  - mvvm
abbrlink: 30063a78
date: 2020-09-25 19:19:26
updated:
categories:
---
复杂的软件必须有清晰合理的架构模式，否则无法开发和维护。

## MVC
作为最出名并且应用最广泛的架构模式，MVC 并没有一个明确的定义，网上流传的 MVC 架构图也是形态各异，可以认为 MVC并没有清晰准确的标准。
{% note info %}
这里主要指视图、控制器和模型三者的交互并没有准确的标准
{% endnote %}

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/mvc.png" width=500 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">一种MVC标准</div>
</center>

MVC中的三者交互虽然不清晰，不过各自的职责还是有明确共识的：
- 视图：管理作为位图展示到屏幕上的图形和文字输出；
- 控制器：占主导地位，翻译用户的输入并依照用户的输入操作模型和视图；
- 模型：可以独立工作，管理应用的行为和数据，响应数据请求（经常来自视图）和更新状态的指令（经常来自控制器）；

## MVP
MVP被描述为 MVC 模式的变种，定义方式也没有统一标准，区别于MVC，M和V不直接通信，它们之间的通信是通过P 来进行的。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/mvp.png" width=500 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">一种MVP标准</div>
</center>

三者的职责：
- Model 定义用户界面所需要被显示的资料模型，一个模型包含着相关的业务逻辑。
- View 视图为呈现用户界面的终端，用以表现来自 Model 的资料，和用户命令路由再经过 Presenter 对事件处理后的资料。
- Presenter 包含着组件的事件处理，负责检索 Model 获取资料，和将获取的资料经过格式转换与 View 进行沟通。

## MVVM
相较于 MVC 和 MVP 模式，MVVM 在定义上就明确得多，相比MVP，MVVM把View和Model的同步逻辑自动化了（数据双向绑定）。
