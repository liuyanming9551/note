# const assertions
const 关键字确保不会发生对变量进行重新分配，并且只保证该字面量的严格类型。
```ts
const x = 'x'; // type 'x'
let y = 'x'; // type string
```
`y`被拓展为更为通用的类型，并允许将其分配给该类型的其他值， 而`x`只能具有`x`的值

用新的 `const` 功能，可以这样做：
```ts
let y = 'x' as const; // type 'x'
```
对象字面量获取只读属性

```ts
const action = {
    type: 'todo', // string
    payload: 'data' // string
}
```
使用 `const` 断言:

```ts
const action = {
    type: 'todo', // todo
    payload: 'data' // data
} as const
```
`type` 和 `paylaod` 都被设置成只读属性，这一特性可以完美的用到 `redux` 中：

```ts
const setCount = (n: number) => {
  return {
    type: 'SET_COUNT',
    payload: n
  } as const
}

const action = setCount(3);
// action has type
//  { readonly type: "SET_COUNT"; readonly payload: number };
```
自定义 hooks 时：

```ts
const useCusHooks = () => {
  const a: string = '1';
  const b: number = 1;
  return [a, b]
} 
// const useCusHooks: () => (string | number)[]

const useCusHooks = () => {
  const a: string = '1';
  const b: number = 1;
  return [a, b]
} as const
// const useCusHooks: () => readonly [string, number]
```
如果不写返回类型，类型推断返回类型为 `const useCusHooks: () => (string | number)[]` ，使用const 断言可以帮助我们推倒出正确的类型