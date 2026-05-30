---
title: RMarkdown和ggplot2
author: 桑峰
date: 2022-07-10T00:00:00.000Z
categories:
  - 可视化
tags:
  - rmarkdown
  - ggplot2
format: hugo-md
---


## R Markdown图表交叉引用

在R Markdown中给图表添加引用是首先需要在文件的输出格式设置为以下三种之一。

``` r
output:
  # bookdown::word_document2: default
  bookdown::html_document2: default
  # bookdown::pdf_document2: default
```

其次在绘制图表时需要添加标签，如下图所示：

<figure>
<img src="img/demo.png" alt="这是一个示例。" />
<figcaption aria-hidden="true">这是一个示例。</figcaption>
</figure>

其中fig-demo为图片的标签。在文中引用时，输入**\\@ref(fig:fig-demo)**即可自动添加图片引用。

## ggplot2添加标签

### 修改坐标轴端点样式

坐标轴端点样式可以通过如下命令修改，下图分别是三种端点样式的是示例图。

``` r
theme(axis.line = element_line(lineend='round'))
```

<figure>
<img src="img/demo-round.png" alt="这是round。" />
<figcaption aria-hidden="true">这是round。</figcaption>
</figure>

<figure>
<img src="img/demo-butt.png" alt="这是butt。" />
<figcaption aria-hidden="true">这是butt。</figcaption>
</figure>

<figure>
<img src="img/demo-square.png" alt="这是square。" />
<figcaption aria-hidden="true">这是square。</figcaption>
</figure>

### 给柱状图添加标签

给柱状图每个柱子添加相应的数字标签可以通过geom_text函数完成，显示效果如下图所示。

``` r
tmpData %>%
  count(MRIAGE_group, Sex) %>%
  ggplot(aes(x = MRIAGE_group, y = n, fill = Sex, label = n)) +
  geom_bar(stat = 'identity', position = position_dodge()) +
  geom_text(position = position_dodge(width = 0.9), vjust = -0.1) +
  labs(x = 'Age', y = 'Number') +
  theme_classic(base_size = 20) +
  theme(
    axis.line = element_line(lineend='round'),
    axis.text.x = element_text(angle = 45, hjust = 0.5, vjust = 0.6))
```

<figure>
<img src="img/demo-label.png" alt="柱状图标签。" />
<figcaption aria-hidden="true">柱状图标签。</figcaption>
</figure>
