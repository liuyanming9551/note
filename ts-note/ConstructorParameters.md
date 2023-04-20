`ConstructorParameters` 类型工具

用来获取构造函数的参数类型

```ts
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age
    }
}

type Pt = ConstructorParameters<typeof Person>

// type Pt = [name: string, age: number]
```