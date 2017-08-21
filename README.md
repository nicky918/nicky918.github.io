### 关于

这是我的个人主页所用的模板，该主题来源于[willard-yuan/willard-yuan.github.io](https://github.com/willard-yuan/willard-yuan.github.io)，你觉得不错也可以fork过去使用

### 主题预览

[Vista's Homepage](https://nicky918.github.io/)。

### 已完成功能

1. 调用豆瓣api显示书单列表
2. 添加latex公式支持([效果见博文](https://nicky918.github.io/blog/decision-tree.html))
3. 添加个人时间线([timeline](https://nicky918.github.io/timeline/))

### 豆瓣

**book**模板，这个你需要有一个豆瓣账号，现在[books/index.html](https://github.com/willard-yuan/willard-yuan.github.io/blob/master/books/index.html)页面把title等修改一下，然后到[douban.api.js](https://github.com/willard-yuan/willard-yuan.github.io/blob/master/js/douban.api.js)把账号user和api改一下。

```js
function DoubanApi() {
	this.defaults = {
		place:"douban",
		user:"57528320",
		api:"08242004429e34bb186c600cc7da9e31",
		book:[{status:"reading",maxnum:20},{status:"read",maxnum:100},{status:"wish",maxnum:100}],
		bookreadingtitle:"在读...",
		bookreadtitle:"读过...",
		bookwishtitle:"想读..."
	};
}
```
