---
title: 评论区没开启
date: 2026-04-10T00:00:00.000Z
disableComments: true
categories:
  - 技术
  - Python
---


- [简介](#简介)
- [数据可视化示例](#数据可视化示例)

## 简介

这是我用 Quarto 编写的内容。

## 数据可视化示例

我们可以直接在博客中运行代码并展示结果：

import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.show()
