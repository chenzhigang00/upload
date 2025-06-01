---
title: Overleaf 局部处理生僻字
date: 2025-05-08 16:22:09
tags: Latex
---

- 文章开头添加以下命令
``` latex
\documentclass[UTF8]{ctexart}
\usepackage{fontspec}
```
- 需要局部换字体的地方，在`{...}`中输入`\CJKfontspec{AR PL UKai CN}`，例如

``` latex
{\CJKfontspec{AR PL UKai CN} 镕} 
```

overleaf上支持的所有中文字体可查看[官网](https://www.overleaf.com/learn/latex/Questions/Which_OTF_or_TTF_fonts_are_supported_via_fontspec%3F#Chinese)

