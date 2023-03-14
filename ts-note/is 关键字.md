is 关键字用来判断是否属于某个类型，可以有效的缩小类型范围
```ts
const isString = (val: unkown): val is String => {
  return typeof val === 'string'
}
```
`isString` 是一个判断传入参数是否为 `String` 类型的函数，用 `is String` 限定了返回值类型，这里估计有人会有这样的疑问：用 `Boolean` 类型也可以限制函数的返回类型。`Boolean` 的确可以限制 `isString` 函数的返回类型，但是用 `is String` 可以更好地缩小类型范围，避免一些隐藏的错误。

当用 `Boolean` 来限制 `isString()` 函数返回类型，以下代码会不会有编译错误，但会有运行错误，因为 `toExponential()` 是 `Number` 类型的一个方法，在 `String` 类型上不存在。

```ts
function example(val:any) {
  if (isString(val)) {
    console.log(val.length)
    console.log(val.toExponential(2))
  }
}

example('test') // Uncaught TypeError: val.toExponential is not a function
```