# extends
`extends` 用法
## 接口继承
```ts
interface A {}
interface B extends A {}
```
B类型继承了A类型
## 条件判断
### 普通用法
```ts
  interface Animal {
    eat(): void
  }
  
  interface Dog extends Animal {
    bite(): void
  }
  
  // A的类型为string
  type A = Dog extends Animal ? string : number
  
  const a: A = 'this is string'
```
如果extends前面的类型能够赋值给extends后面的类型，那么表达式判断为真，否则为假

### 范型用法
+ 分配条件类型
  
```ts
  type A1 = 'x' extends 'x' ? string : number; // string
  type A2 = 'x' | 'y' extends 'x' ? string : number; // number
  
  type P<T> = T extends 'x' ? string : number;
  type A3 = P<'x' | 'y'> // ?
```
看起来`A3`的类型应该是 `number`,但正确答案是 `string | number`,这就是分配条件类型

`When conditional types act on a generic type, they become distributive when given a union type`

对于使用`extends`关键字的条件类型（即上面的三元表达式类型），如果`extends`前面的参数是一个泛型类型，当传入该参数的是联合类型，则使用分配律计算最终的结果。分配律是指，将联合类型的联合项拆成单项，分别代入条件类型，然后将每个单项代入得到的结果再联合起来，得到最终的判断结果。

该例中，`extends`的前参为`T`，`T`是一个泛型参数。在A3的定义中，给`T`传入的是`x`和`y`的联合类型`'x' | 'y'`，满足分配律，于是`x`和`y`被拆开，分别代入`P<T>`

`P<'x' | 'y'> => P<'x'> | P<'y'>`

`x`代入得到

`'x' extends 'x' ? string : number => string`

`y`代入得到

`'y' extends 'x' ? string : number => number`

然后将每一项代入得到的结果联合起来，得到`string | number`

总之，满足两个要点即可适用分配律：第一，参数是泛型类型，第二，代入参数的是联合类型
+ 特殊的`never`

```ts
  // never是所有类型的子类型
  type A1 = never extends 'x' ? string : number; // string

  type P<T> = T extends 'x' ? string : number;
  type A2 = P<never> // never
```
上面的示例中，`A2`和`A1`的结果竟然不一样，看起来`never`并不是一个联合类型，所以直接代入条件类型的定义即可，获取的结果应该和A1一直才对啊？

实际上，这里还是条件分配类型在起作用。`never`被认为是空的联合类型，也就是说，没有联合项的联合类型，所以还是满足上面的分配律，然而因为没有联合项可以分配，所以`P<T>`的表达式其实根本就没有执行，所以`A2`的定义也就类似于永远没有返回的函数一样，是`never`类型的。


+ 防止条件判断中的分配

```ts
  type P<T> = [T] extends 'x' ? string : number;
  type A1 = P<'x' | 'y'> // number
  type A2 = P<never> // string
```
在条件判断类型的定义中，将泛型参数使用[]括起来，即可阻断条件判断类型的分配，此时，传入参数`T`的类型将被当做一个整体，不再分配。

```ts
interface Pro {
    name: string;
    price: number;
    count: number;
}

type Pkeys = keyof Pro // 这里是推断不出具体的key类型

type AllKeys<T> = T extends any ? T : never;
type PAllKeys = AllKeys<keyof Pro> // 'name' | 'price' | 'count'
```

