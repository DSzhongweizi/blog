---
title: NPM——Node包管理器
date: 2018-01-05 00:00:00
---
`npm` 是Node的开放式模块登记和管理系统，是Node.js包的标准发布平台，用于Node.js包的发布、传播、依赖控制，其提供的命令行工具，可以方便地下载、安装、升级、删除包，也可以让你作为开发者发布并维护包。

{% tabs Mrakdown %}
<!-- tab 基础命令 -->
- `npm i npm -g`：升级本身
- `npm help`：查看 npm 命令列表
- `npm -l`：查看各个命令的简单用法
- `npm -v`：查看 npm 的版本
- `npm config list -l`：查看 npm 的配置
- `npm init [command]`：初始化 `package.json` 文件
  - `-y`：yes
  - `-f`：force
<!-- endtab -->
<!-- tab 常用命令 -->
### 区别生产和开发环境
- `devDependencies`：开发依赖，适用于本地开发环境，多见于开发工具库，不会被打入包内
- `dependencies`：生产依赖，适用于生产环境

通过 `NODE_ENV=[developement|production]` 可以指定
### 安装
`npm i <package name> [command]`：
- `无命令`：将一个模块下载到当前项目的`node_modules`子目录，然后只有在项目目录之中，才能调用这个模块
- `-g`：将一个模块安装到系统目录中，各个项目都可以调用，适用于工具模块，比如`eslint`和`gulp`
- `--save`：安装生产依赖
- `-save-dev`：安装开发依赖

依据`package.json`文件安装对应版本，或者安装最新版本

### 更新
`npm update [command]`：
- `-g`：更新全局包

在 package.json 文件所在的目录中执行更新

### 卸载
`npm uninstall <package> [command]`
- `-g`：卸载全局包
- `--save`：从 package.json 文件中删除依赖
- `-save-dev`：删除 `devDependency` 依赖

<!-- endtab -->
<!-- tab 其他 -->
<!-- endtab -->
{% endtabs %}


