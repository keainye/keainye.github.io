---
title: 算法常用代码段
date: 2023/02/28
---
此页面包含了一些算法题常用的代码段，供我随处复制。

## 宏定义

从上至下使用频率升高

```c
#define d(i, s, e) for (int i = s; i >= e; i--)
#define f(i, s, e) for (int i = s; i < e; i++)
#define abs(a) ((a) > 0 ? (a) : -(a))
#define min(a, b) ((a) < (b) ? (a) : (b))
#define max(a, b) ((a) > (b) ? (a) : (b))
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

二分查找

```c
bool bis(int *src, int s, int e, int tgt) {
  if (s > e) return 0;
  if (s == e) return src[s] == tgt;
  int mid = (s+e)/2;
  if (src[mid] == tgt) return 1;
  return tgt < src[mid] ? bis(src, s, mid-1, tgt) : bis(src, mid+1, e, tgt);
}
```

strsplt 字符串分割

```c
void strcpy1(char* dst, char* src, int ss, int l) {
  f(i, 0, l) dst[i] = src[ss + i];
  dst[l] = 0;
}

char** strsplt(char* src, int* n) {
  int l = strlen(src), s = 0;
  *n = 1;
  f(i, 0, l) if (src[i] == ' ')(*n)++;
  char **ret = (char**)malloc(sizeof(char*) * (*n)), *newstr;
  *n = 0;
  f(i, 0, l) {
    if (src[i] != ' ') continue;
    newstr = (char*)malloc(sizeof(char) * (i - s + 1));
    strcpy1(newstr, src, s, i - s);
    ret[(*n)++] = newstr;
    s = i + 1;
  }
  newstr = (char*)malloc(sizeof(char) * (l - s + 1));
  strcpy1(newstr, src, s, l - s);
  ret[(*n)++] = newstr;
  return ret;
}
```
