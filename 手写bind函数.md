# 手写bind方法

```js
Function.prototype._bind = function(target) {
	return () => {
	  this.call(target)
	}
}
  
function fn () {
	console.log(this)
}

let _fn = fn._bind({name: 'zhangsan'})
```