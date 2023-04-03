# `typeof` 操作符

用来获取ts定义的类型

```ts
interface Person {
    name: string;
    age:number;
}

const p: Person = {
    name: '小明',
    age: 18
}
type People = typeof p; // type People = Person

```
也可以用来获取函数对象的类型

```ts
function toArray(x: number): Array<number> {
  return [x];
}

type Func = typeof toArray; // -> (x: number) => number[]
```
