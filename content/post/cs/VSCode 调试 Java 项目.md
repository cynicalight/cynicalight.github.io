---
title: 'VSCode 调试 Java 项目'
date: 2025-01-23T23:16:31+08:00
lastmod: 2025-01-23T23:16:31+08:00
draft: false
slug: 8286daf7
---


### 安装插件

VSCode 搜 Java，直接无脑把第一页的 Java 插件全装上，省事。

![200](../../../../img/Pasted%20image%2020250123164530.png)


### 配置 launch.json

来到运行和调试界面：

![300](../../../../img/Pasted%20image%2020250123164756.png)

建议创建 `launch.json` 文件，并配置。

默认配置如下：

![500](../../../../img/Pasted%20image%2020250123165119.png)

其中 config 中每个对象分别对应左侧`运行和调试`栏顶部的一个选项：

![500](../../../../img/Pasted%20image%2020250123165244.png)

接下来我们添加一个config项 `Launch with Arguments Prompt`：
- 这个选项可以在调试的时候输入参数，非常必要

![500](../../../../img/Pasted%20image%2020250123165350.png)

直接在空白处输入 java 就会弹出选项，VSCode 会自动补全你想要的配置：

![500](../../../../img/Pasted%20image%2020250123165824.png)

之后就可以正常下断点，愉快的开始调试了。






