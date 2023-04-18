# `Object.getOwnPropertyDescriptor()`

方法返回制定对象上的一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

```js
const o = {
    get foo() {
        return 17
    }
}

const d = Object.getOwnPropertyDescriptor(o, 'foo')

// d {
//   configurable: true,
//   enumerable: true,
//   get: /*the getter function*/,
//   set: undefined
// }
```
注意： 在es5中，如果该方法第一个参数不是对象，会抛出`TypeError`,在es6中，则会强制转成对象

```js
Object.getOwnPropertyDescriptor('foo', 0);
// 类型错误："foo" 不是一个对象  // ES5 code

Object.getOwnPropertyDescriptor('foo', 0);
// Object returned by ES2015 code: {
//   configurable: false,
//   enumerable: true,
//   value: "f",
//   writable: false
// }
```

上面的`'foo'`是一个字符串，转成对象是 `{ 0: 'f', 1: 'o', 2: 'o' }`, 故而属性`0`的`value`是`f` 
