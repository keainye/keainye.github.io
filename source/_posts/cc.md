---
title: C++ 中那些奇怪的操作
---
> 最近在上手墨干编辑器（Mogan）。作为一个 GNU 社区的项目，Mogan 代码中使用了许多我以前从未见过的 C++ 操作。在此记录。

**调用类的构造函数而不产生新对象**

通常情况下，如果像这样分配内存：

```C
object *obj = (object *) malloc(sizeof(object));
```

编译器不会调用 `object` 的构造函数。

但是有时候，我们需要先分配内存，做一些处理（比如检查内存是否充足）后再调用其构造函数。在这种情况下，`new object` 这种代码并不能满足我们的需求。

在 Mogan 源码中学到如下操作：

```C++
object *obj = (object *) malloc(sizeof(object));
new (obj) object;
```

这样就可以调用 object 的构造函数而不分配新的内存。

`new (obj) object` 是一个有返回值的操作，返回值就是 `obj`。

```C++
object *obj = (object *) malloc(sizeof(object));
auto a = new (obj) object;
if (a == obj) cout << "equal" << endl;
delete a;

// output:
// constructor
// equal
// deconstructor
```
