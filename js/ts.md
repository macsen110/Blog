# typescript 高级技巧

一. 结构类型系统(Structural Type System)

1. 交集

```
type Name = {
    name: string
}

type Point = {
    x: number;
    y: number
}

type NamedPoint = Name & Point

function superPoint(point: PointName) {
    console.log(point.x) //ok
    console.log(point.name) // ok
    console.log(point.xxx) //wrong
}
```

2. 合集(Union Type, 联合类型)

合集类型既可以是类型,也可以是字面量

```
type NameOrPoint = Name | Point;

type MyBoolean = true | false;

type Result = { status: 'ok' } | { status: 'error', reason: string }

```

3. 类型收缩(Type Narrowing)

   3.1 字面量类型(literal Type)

   > 字面量限定的不再是一个类似 string 的一个范围，而是具体的单值, 又称为单例类型(singleton type)

   ```
   type GreetName = 'world' | 'moto';
   function greet(name: GreetName) {
       return `hello, ${name}!`
   }

   greet('world'); //ok
   greet('moto'); //ok
   greet('ccc') // Error

   ```

   > 每一个合集中的构成类型都有一个同名不同值的单例类型属性，保证它们之间没有任何交集，然后通过在该属性上应用类型唯一区分。

   ```

   type Square = {
   kind: 'square';
   size: number;
   };

   type Rectangle = {
   kind: 'rectangle';
   width: number;
   height: number;
   };

   type Circle = {
   kind: 'circle';
   radius: number;
   }

   type Shape = Square | Rectangle | Circle;

   function area(shape: Shape): number {
   switch (shape.kind) {
       case 'square':
       return shape.size * shape.size;
       case 'rectangle':
       return shape.width * shape.height;
       case 'circle':
       return Math.PI * shape.radius * shape.radius;
   }
   }
   ```

4. 类型编程

   4.1 >泛型是类型的函数 \<T>(T 代表 types)

5. key words

   5.1 keyof 类似于 object.keys()

   5.2 is 判断值的类型

   5.3 typeof 代表取某个值的 type

   5.4 declear 声明全局

   5.5 emun
