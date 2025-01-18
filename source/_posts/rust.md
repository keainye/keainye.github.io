---
title: Rust 笔记
date: 2023/04/04
hide: true
---
### 所有权

所有权原则：一个值只能被一个变量所有，离开该变量作用域后值被抛弃。

栈对象（基本数据类型）的赋值是直接拷贝，所有权不丢失。

堆对象（非基本数据类型）的赋值是移交堆指针，故丢失所有权。

`for item in collection` 丢失所有权的 for

`for item in &collection` 不可变借用

`for item in &mut collection` 可变借用
