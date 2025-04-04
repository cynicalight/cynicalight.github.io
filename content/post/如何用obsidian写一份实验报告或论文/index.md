---
title: 如何用obsidian写一份实验报告或论文
date: 2025-04-04
tags:
    - default
categories:
    - default
---
  

> 不是word不会，就是喜欢obsidian写 =)

  

## 0.1 优雅的报告封面

  

用html仿照word的格式，熟练了还是很方便的。

  

常用html语法：

```html

<h1 style="font-size: 30px; text-align: center; color: #111111;margin-top: 100px; margin-bottom: 0px;">学院</h1>

  

<h3 style="font-size: 29px; text-align: center; font-family: 'KaiTi'; font-weight: bold;color: #111111; margin-bottom: 100px;margin-top: 100px;">操作系统设计与实践期末综合实验</h3>

  

<p style="font-size: 18px; margin-left: 240px; text-align: left; color: #111111; margin-bottom: 100px; line-height: 2.5;">

专 业 名 称   ：信息安全       

<br>

课 程 名 称   ：操作系统设计与实践

<br>

学 生 姓 名   ：44

</p>

  

<p style="font-size: 22px; width: fit-content; margin: 0 auto; text-align: left; color: #111111; margin-bottom: 0px;">二○二四年十二月</p>

  

```

  

封面效果：

![11](Pasted%20image%2020241212205037.png)

  
  

> 20241217
>
> 正文用了css样式，强制修改了p和h1h2的样式，但是h6一般不用到，所以我在css样式中把h6的设置删去，封面可以全部用h6写，具体字体样式大小等可以像原来一样使用html语法设置。

  
  

---

## 0.2 加上多级编号

插件1 serial：

![](Pasted%20image%2020241212165438.png)

插件2 number title：

![](Pasted%20image%2020241212165433.png)

效果：

![](Pasted%20image%2020241212205102.png)

  

---

## 0.3 生成目录

  

插件：table of content

![](Pasted%20image%2020241212165425.png)

  

- 注意：生成位置前面不能有任何标题

  

---

## 0.4 导出pdf

插件： better pdf

![](Pasted%20image%2020241212164659.png)

  

- 优点：pdf目录点击能够跳转，可以加页脚

  

---

## 0.5 补充：css样式设置内容部分的报告/论文样式

- 参考：
	- [【新手向教程】OB样式调整基础课：CSS入门科普 - 经验分享 - Obsidian 中文论坛](https://forum-zh.obsidian.md/t/topic/43996)
	- [obsidian 有无适合中文学术/出版风格的主题？ - 疑问解答 - Obsidian 中文论坛](https://forum-zh.obsidian.md/t/topic/2994/3)

稍作修改，给样式加上了类名 `paper`，当需要该样式时，只要在文件的properties中指定一个`cssclasses：paper` 即可。
的property：
![](Pasted%20image%2020241217142324.png)

预览可见样式改变：
![](Pasted%20image%2020241217142550.png)


稍作修改后的paper.css
```css
/* ---------------修改字体使css可以与Mac系统通用 ---------------- */
/* 编辑者：Dexter                                               */
/* 编辑日期：20220908                                          */
/* 主要工作：                                                  */
/*       宋体 替换为PostScriptName格式：宋体-简,                */
/*       黑体 替换为PostScriptName格式：黑体-简                  */
/*       楷体 替换为PostScriptName格式：KaiTi                  */
/*   Times New Roman 替换为PostScriptName格式：TimesNewRomanPSMT */
/* ------------------------------------------------------------ */

/* ------------------------------------------------------------ */
/* 原css作者：乔诚                                               */
/* 链接：https://forum-zh.obsidian.md/t/topic/2994/3            */
/* 修改：bu44er                                               */
/* 修改日期: 20241217                                             */
/* ------------------------------------------------------------ */

.paper {
  box-shadow: none !important;
  background-color: #fff !important;
}


.paper p, 
.paper ul, 
.paper ol, 
.paper h1, 
.paper h2, 
.paper h3, 
.paper h4, 
.paper h5, 
.paper h6, 
.paper a, 
.paper strong, 
.paper sup, 
.paper .footnote-link, 
.paper .image-embed, 
.paper blockquote {
  color: #000000 !important;
}

.paper strong {
  font-family: "TimesNewRomanPSMT","黑体-简";
  font-weight: bold;
}

.paper p {
  font-family: "TimesNewRomanPSMT","宋体-简" !important;
  text-decoration-line: none;
  margin: 0px 0px;
  line-height: 23pt;
  font-size: 17px;
  text-indent: 0em;
}

.paper h1 {
  font-family: "TimesNewRomanPSMT","黑体-简" !important;
  font-size: 24px !important;
  text-align: center !important;
  font-weight: bold !important;
}

.paper h2 {
  font-family: "TimesNewRomanPSMT","黑体-简" !important;
  font-weight: 700;
  font-size: 21px !important;
  text-align: left;
  font-weight: bold !important;
}

.paper h3 {
  font-family: "TimesNewRomanPSMT","黑体-简" !important;
  font-weight: 700;
  font-size: 19px !important;
  text-align: left;
  font-weight: bold !important;
}

.paper h4 {
  font-family: "TimesNewRomanPSMT","黑体-简" !important;
  font-weight: 700;
  font-size: 18px !important;
  text-align: left;
  font-weight: bold !important;
}

.paper h5 {
  font-family: "TimesNewRomanPSMT","宋体-简" !important;
  font-size: 20px !important;
  text-align: left !important;
  font-weight: bold;
}

.paper .footnote-link {
  color: #2E3338;
  line-height: 10px !important;
}

.paper sup {
  vertical-align: 25%;
  font-size: 10px;
}

.paper .image-embed {
  display: flex;
  align-items: center;
  justify-content: center;
}

.paper ul,
.paper ol {
  font-family: "TimesNewRomanPSMT","宋体-简" !important;
  text-decoration-line: none;
  margin: 0px 0px;
  padding-left: 0;
  margin-left: 0;
  line-height: 23pt;
  font-size: 16px;
  text-indent: 0em !important;
}

.paper .markdown-preview-view table {
  width: 100%;
  margin-top: 12px;
}

.paper .markdown-preview-view td,
.paper .markdown-preview-view th {
  border: none;
  font-size: 14px;
  padding: 4px 10px;
}

.paper .theme-light .markdown-preview-view th {
  border-bottom: 1px solid #272727;
  border-top: 1px solid #272727;
  font-weight: 800;
  background-color: #00000040;
}

.paper .theme-light .markdown-preview-view tr:nth-child(odd) {
  background-color: #00000030;
}

.paper .theme-light .markdown-preview-view tr:last-child {
  border-bottom: 1px solid #272727;
}

.paper .theme-dark .markdown-preview-view th {
  border-bottom: 2px solid #666666;
  border-top: 2px solid #999999;
  font-weight: 800;
}

.paper .theme-dark .markdown-preview-view tr:nth-child(odd) {
  background-color: #ffffff30;
}

.paper .theme-dark .markdown-preview-view tr:nth-child(even) {
  background-color: #ffffff20;
}

.paper .theme-dark .markdown-preview-view tr:last-child {
  border-bottom: 1px solid #999999;
}

.paper .markdown-preview-view blockquote th {
  vertical-align: bottom;
}

.paper .markdown-preview-view blockquote th,
.paper blockquote tr {
  background-color: #00000000 !important;
  border: none !important;
}

.paper blockquote tr td {
  vertical-align: top;
}

.paper blockquote table tbody tr:first-child {
  font-family: "TimesNewRomanPSMT","宋体-简" !important;
  font-weight: bold;
  text-align: center;
}

.paper blockquote {
  border: none !important;
  padding: 0px;
  margin: 20px 50px;
}

.paper blockquote p {
  font-family: "KaiTi","TimesNewRomanPSMT";
}

.paper .theme-light blockquote p {
  color: #5f5f5f;
}

.paper .theme-dark blockquote p {
  color: #a1a1a1;
}

/* .paper pre {
  border: solid 1px #000;
}

.paper code[class*="language-"] {
  border-left: solid 5px #888 !important;
  border-radius: 0px;
  line-height: 1 !important;
} */

.paper .markdown-preview-view hr {
  border: none;
  border-top: 1px solid;
  border-color: #000;
}
```
