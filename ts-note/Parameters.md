# `Parameters` 类型工具

用于获取函数中的参数类型

```ts
type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never
```

```ts
type P1 = Parameters<(name:number)=>void>
// type P1 = [name: number]

type P2 = Parameters<<T>(arg:T) => T>
// type P2 = [arg: unknown]

function fn(x: number, y: number, z: number) {
    return x + y + z
}

type FnType = typeof fn;

// type FnType = (x: number, y: number, z: number) => number
type p3 = Parameters<FnType>
// type P3 = [x: number, y: number, z: number]
```