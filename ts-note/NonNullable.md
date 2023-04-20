# `NonNullable` 类型工具

`NonNullable`用于将类型中的`null`，`undefined`属性移除掉

```ts
type NonNullable<T> = T & {}
```
`null`和`undefined`在ts中被成为可选类型（optional type），`T `与空对象类型进行交叉，会使得新的类型中包含了空对象类型的所有属性和方法。而空对象类型没有任何属性或方法，因此新的类型中也不会包含任何额外的属性或方法。这就相当于将类型 T 中的所有可选属性（例如 `null` 或 `undefined` 类型的属性）都转换为了必选属性