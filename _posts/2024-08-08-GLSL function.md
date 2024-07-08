---
title: GlSL常用函数算法
date: 2024-07-08 14:11:00 +0800
categories: [GLSL, Shader]
tags: [glsl]    
---

# 常用函数算法
- 一些平时自己经常用的函数算法

## 淡入淡出函数
```glsl
lowp float fadeInOut(lowp float t) {
// 淡入淡出效果
return smoothstep(0.0, 0.5, t) * (1.0 - smoothstep(0.5, 1.0, t));
}  
```

