# qixi-confession

可定制的七夕表白文字

因为有需求，所以封装了一个canvas绘制七夕表白文字的效果。

![qixi](./qixi.gif)


```
(async () => {
	await startDrawTextAndHeart(container, heartCan, 200, 350, '一直没有明确告诉你');
	await startDrawTextAndHeart(container, heartCan, 300, 300, '遇见你是多么美好的事情');
})();
```

目前定制就改这里好了，只需要改后面的文字，和第三个参数y，也可以调整下第四个参数x

## 实现思路：

[使用canvas定制你的七夕表白文字](https://juejin.im/editor/drafts/5d4aa3cef265da03bd050681)

## 安装依赖
```
yarn install
```

### 开发
```
yarn run serve
```

### 构建
```
yarn run build
```