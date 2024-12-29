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

## UV旋转
```glsl
//弧度
void Rotate_Radians(vec2 UV, vec2 Center, float Rotation, out vec2 Out)
{
  UV -= Center;
  float s = sin(Rotation);
  float c = cos(Rotation);
  mat2 rMatrix = mat2(vec2(c, -s), vec2(s, c));
  rMatrix *= 0.5;
  rMatrix += 0.5;
  rMatrix = rMatrix * 2.0 - 1.0;
  UV.xy = UV.xy*rMatrix;
  UV += Center;
  Out = UV;
}
```

```glsl
//角度
void Rotate_Degrees(vec2 UV, vec2 Center, float Rotation, out vec2 Out)
{
  Rotation = Rotation * (3.1415926f/180.0f);//转角度
  UV -= Center;
  float s = sin(Rotation);
  float c = cos(Rotation);
  mat2 rMatrix = mat2(vec2(c, -s), vec2(s, c));
  rMatrix *= 0.5;
  // 在这里添加GLSL编辑器的HTML代码
<div id="glsl-editor-container"></div>
<script src="assets/js/glslEditor.js"></script>
<script>
// 初始化GLSL编辑器
const editor = new glslEditor('#glsl-editor-container');
  </script>
  rMatrix += 0.5;
  rMatrix = rMatrix * 2.0 - 1.0;
  UV.xy = UV.xy*rMatrix;
  UV += Center;
  Out = UV;
}
```

## UV中心缩放
```glsl
// 顶点着色器
vertex shader

in vec2 uv;  // 输入的 UV 坐标

out vec2 scaled_uv;  // 缩放后的 UV 坐标

void main() {
    vec2 center_uv = vec2(0.5, 0.5);  // UV 中心的坐标（可以根据需要调整）

    vec2 relative_uv = uv - center_uv;  // 相对 UV 坐标

    relative_uv *= 2.0;  // 进行缩放操作（可以根据需要调整缩放因子）

    scaled_uv = relative_uv + center_uv;  // 加上中心坐标得到最终的缩放后的 UV 坐标
}
```

