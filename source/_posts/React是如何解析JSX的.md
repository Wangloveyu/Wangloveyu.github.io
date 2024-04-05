---
title: React是如何解析JSX的
date: 2024-03-31 02:20:35
tags:
---









## 整体调用栈

```mermaid
graph TD;
A[遇见JSX] --> createElementWithValidation;
createElementWithValidation-->isValidElementType[isValidElementType,校验];
createElementWithValidation-->createElement
```

