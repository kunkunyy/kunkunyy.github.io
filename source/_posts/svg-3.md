---
title: SVG 学习笔记（三）基础形状
date: 2023-11-08 20:50:11
tags:
- SVG 学习
categories:
- [前端学习笔记]
---

```xml
<!--椭圆-->
<svg width="200" height="250" version="1.1" xmlns="http://www.w3.org/2000/svg">
<!--矩形  起始位置坐标    设置圆角的半径           宽高              线条颜色          填充色              线条宽度     -->
  <rect x="60" y="10" rx="10" ry="10" width="30" height="30" stroke="black" fill="transparent" stroke-width="5"/>
<!--圆          圆心坐标      半径     线条颜色         填充色             线条宽度    -->
  <circle cx="25" cy="75" r="20" stroke="red" fill="transparent" stroke-width="5"/>
<!--椭圆        圆心坐标      x半径   y半径     线条颜色          填充色            线条宽度    -->
  <ellipse cx="75" cy="75" rx="20" ry="5" stroke="red" fill="transparent" stroke-width="5"/>
<!--直线    起始点坐标         结束位置坐标       线条颜色           线条宽度   -->
  <line x1="10" x2="50" y1="110" y2="150" stroke="orange" stroke-width="5"/>
<!--折线  点集  线条颜色  填充色  线条宽度-->
  <polyline points="60, 110 65, 120 70, 115 75, 130 80, 125 85, 140 90, 135 95, 150 100, 145"
      stroke="orange" fill="transparent" stroke-width="5"/>
<!--多边形  点集  线条颜色 填充色  线条宽度-->
  <polygon points="50, 160 55, 180 70, 180 60, 190 65, 205 50, 195 35, 205 40, 190 30, 180 45, 180"
      stroke="green" fill="transparent" stroke-width="5"/>
<!--路径 -->
  <path d="M20,230 Q40,205 50,230 T90,230" fill="none" stroke="blue" stroke-width="5"/>
</svg>
```

# 路径

* 路径除了基础属性`fill、stroke、stroke-width`等以外其最为重要的就是`d`

| 用处 | 参数 | 说明 |
| --- | --- | --- |
| 起始位置| `M(x, y)` | 开始位置坐标 |
| 直线 | `L(x, y)` | 当前位置绘制直线到(x, y) |
| 水平直线 | `H(x)` | 当前位置绘制水平直线至x |
| 垂直直线 | `V(y)` | 当前位置绘制垂直直线至y |
| 闭合 | `Z` | 闭合开始结束位置 |
| 三次贝塞尔曲线 | `C x1 y1, x2 y2, x y` | 控制点1(x1 y1) 控制点2(x2 y2) 曲线终点(x y) |
| 三次贝塞尔曲线（简写） | `S x2 y2, x y` | 必须跟在C/S后面，控制点2(x2 y2) 曲线终点(x y) |
| 两次贝塞尔曲线 | `Q x1 y1, x y` | 控制点(x1 y1) 曲线终点(x y) |
| 两次贝塞尔曲线（简写） | `T x y` | 必须跟在Q/T后面，曲线终点(x y) |
| 弧形 | `A rx ry x-axis-rotation large-arc-flag sweep-flag x y` | x半径 y半径 x轴偏移角度 选择弧线大(1)小(0) 画弧顺(1)逆(0)时针 终点(x y) |