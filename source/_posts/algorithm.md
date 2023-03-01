---
title: 算法常用代码段
date: 2023/02/28
---

此页面包含了一些算法题常用的代码段，供我随处复制。

## 宏定义

从上至下使用频率升高

```c
#define ui(n) for (unsigned i = 0; i < n; i++)
#define ids(s, e) for (int i = s; i >= e; i--)
#define id(n) for (int i = n; i >= 0; i--)
#define is(s, e) for (int i = s; i < e; i++)
#define j(n) for (int j = 0; j < n; j++)
#define i(n) for (int i = 0; i < n; i++)
#define abs(a) (a > 0 ? a : -a)
#define min(a, b) (a < b ? a : b)
#define max(a, b) (a > b ? a : b)
```

## 函数

堆排序（不使用外部空间）

```c
void heap_sort(int *src, int n) {
  int p, t;
  for (int i = 1; i <= n; i++) {
    p = i;
    while (p/2 > 0 && src[p/2-1] < src[p-1]) {
      t = src[p/2-1];
      src[p/2-1] = src[p-1];
      src[p-1] = t;
      p /= 2;
    }
  }
  for (int i = n-1; i >= 0; i--) {
    t = src[0];
    src[0] = src[i];
    src[i] = t;
    p = 2;
    while (p-1 < i) {
      if (p < i && src[p-1] < src[p]) p++;
      if (src[p/2-1] > src[p-1]) break;
      t = src[p/2-1];
      src[p/2-1] = src[p-1];
      src[p-1] = t;
      p *= 2;
    }
  }
}
```