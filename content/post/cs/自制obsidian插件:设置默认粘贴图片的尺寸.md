---
title: '自制obsidian插件 Image Size: 设置默认粘贴图片的尺寸'
date: 2024-12-09T14:15:03+08:00
lastmod: 2024-12-09T14:15:03+08:00
draft: false
slug: 74665601
tags: ["obsidian"]
---

- github：[GitHub - cynicalight/obsidian-image-size: Set the default size for pasted images.](https://github.com/cynicalight/obsidian-image-size)
- 参考：[Build a plugin - Developer Documentation](https://docs.obsidian.md/Plugins/Getting+started/Build+a+plugin)

obsidian默认粘贴的图片的宽度总是占满笔记宽度的，我在粘贴一些网课的ppt的时候总是感觉这种默认设置显得图片太大了，当然，我可以直接修改图片的md链接在方括号中写一个大小，比如400，但是每次改太麻烦了，我希望有一个默认粘贴图片宽度的预设，于是写了这个简单的插件 Image Size，真的非常简单...

实现这一目的的主要过程就是以下两步：

1. 阻止系统的默认粘贴：
```ts
// 阻止默认粘贴行为
if (items) {
	for (let i = 0; i < items.length; i++) {
		const item = items[i];
		if (item.type.startsWith("image/")) {
			evt.preventDefault();
```

2. 调用obsidian的editor自己插入一个图片（md语句）：
```ts
// 插入 Markdown 图片链接
const cursor = editor.getCursor();
const markdownImage = `![${this.settings.imageSize}](${imageNameMd})`;
editor.replaceRange(markdownImage, cursor);
```
- 由于阻止了默认的粘贴，所以我需要再写一个保存图片文件到设置的附件文件夹的操作，这里就不展示了。


---
## 20240320

在提交社区review的时候，按照他们的要求改了挺多的，详见github。

