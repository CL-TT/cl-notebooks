# 使用移动端uView组件库遇到的问题

## 1. uni-app中的rpx单位和px单位的转换

1. uview中的组件库大多数都是用的px，不具备响应式

```js
/**
 * 把rpx转换成px, uview组件中大多数都是px单位，在不同机型适配不友好，需要一个转换
 * @param {Object} rpx
 */
export const rpxToPx = (rpx) => {
	const screenWidth = uni.getSystemInfoSync().screenWidth
	return (rpx * Number.parseInt(screenWidth)) / 750
}


<u-tabs :lineHeight="rpxToPx(6)"></u-tabs>
```

