## ES6特性

### Object属性和方法

#### Object.defineProperty(obj, prop, descriptor

`**Object.defineProperty()**` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

+ obj: 要在其上定义属性的对象。
+ prop: 要定义或修改的属性的名称。
+ descriptor: 将被定义或修改的属性描述符。
+ 返回值: 被传递给函数的对象。



属性描述符：分为**数据描述符**和**存取描述符**，描述符必须是这两种形式之一，不能同时是两者。

|    属性名    |                 类型                 |                             说明                             |
| :----------: | :----------------------------------: | :----------------------------------------------------------: |
| configurable | 数据描述符和存取描述符均可具有此属性 | 当且仅当该属性的 configurable 为 true 时，该属性`描述符`才能够被改变，同时该属性也能从对应的对象上被删除。**默认为 false**。 |
|  enumerable  | 数据描述符和存取描述符均可具有此属性 | 当且仅当该属性的`enumerable`为`true`时，该属性才能够出现在对象的枚举属性中，即可以使用`for in和Object.keys()`。**默认为 false**。 |
|    value     |              数据描述符              | 该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。**默认为 undefined**。 |
|   writable   |              数据描述符              | 当且仅当该属性的`writable`为`true`时，`value`才能被[赋值运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Assignment_Operators)改变。**默认为 false**。 |
|     get      |               存取描述               | 一个给属性提供 getter 的方法，如果没有 getter 则为 `undefined`。当访问该属性时，该方法会被执行，方法执行时没有参数传入，但是会传入`this`对象（由于继承关系，这里的`this`并不一定是定义该属性的对象）。 |
|     set      |               存取描述               | 一个给属性提供 setter 的方法，如果没有 setter 则为 `undefined`。当属性值修改时，触发执行该方法。该方法将接受唯一参数，即该属性新的参数值。 |

## 优秀源码

### 缓存函数

```
function cached<F: Function> (fn: F): F {
  const cache = Object.create(null)
  return (function cachedFn (str: string) {
    const hit = cache[str]
    return hit || (cache[str] = fn(str))
  }: any)
}
```

> 此函数的功能是将传入的fn执行结果缓存起来，下次如果给`cachedFn`函数传入了相同的参数，那就直接取缓存中的结果就好了

下面是一个应用的实例：

```
const idToTemplate = cached(id => {
  const el = query(id)
  return el && el.innerHTML
})
```

### 将二维数组转成一维数组

```
let arr = [[1,2,3], [2,3]]
console.log(Array.prototype.concat.apply([], arr))
```

> 输出`[1, 2, 3, 2, 3]`，这里巧妙地运用了`apply`，将arr作为参数数组传入。

### 一次性函数

使函数只执行一次

```
function once(func) {
	let called = false
	return function() {
		if(!called) {
			func.apply(this, arguments)
			called = true
		}
	}
}

let i = 1
function exec() {
	console.log(i++)
}

let test = once(exec)
test()
test()
test()
```

> 只输出1，重点是once函数，将真正要执行的函数通过once函数代理一层去控制它只执行一次。



