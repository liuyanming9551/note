# `ReturnType` 类型工具
获取函数的返回值类型

```ts
function f1(name: string) {
    return name;
}

type F1Type = ReturnType<typeof f1>
```