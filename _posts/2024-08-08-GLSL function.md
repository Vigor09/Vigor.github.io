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
float fadeInOut(lowp float t) {
// 淡入淡出效果
return smoothstep(0.0, 0.5, t) * (1.0 - smoothstep(0.5, 1.0, t));
}  
```
---
## UV旋转
```glsl
//角度
vec2 Rotate_Degrees(vec2 UV, vec2 Center, float Rotation) {
    Rotation = radians(Rotation);  // 角度转弧度
    UV -= Center;
    float s = sin(Rotation);
    float c = cos(Rotation);
    UV = vec2(
        UV.x * c - UV.y * s,
        UV.x * s + UV.y * c
    );
    UV += Center;
    return UV;
}
```
---
## UV中心缩放
vertex shader
```glsl
vec2 UVScale(vec2 uv, float scaleFactor, bool clampToEdge) {
    vec2 center = vec2(0.5);
    vec2 relativeUV = uv - center;
    relativeUV *= scaleFactor;
    vec2 result = relativeUV + center;
    
    return result;
}
```
---
# PS中的常见效果算法函数
##  叠加混合
```glsl
// 叠加混合模式的实现
vec3 overlayBlend(vec3 base, vec3 blend) {
    // 根据基色值执行不同的混合计算
    return mix(
        2.0 * base * blend,
        1.0 - 2.0 * (1.0 - base) * (1.0 - blend),
        step(0.5, base)
    );
}

// 带透明度的叠加混合模式
vec4 overlayBlendWithAlpha(vec4 base, vec4 blend) {
    // 计算叠加混合后的RGB值
    vec3 rgb = overlayBlend(base.rgb, blend.rgb);

    // 计算最终的透明度
    float alpha = base.a + blend.a * (1.0 - base.a);

    // 考虑透明度的最终颜色
    return vec4(mix(base.rgb, rgb, blend.a * (1.0 - base.a) / alpha), alpha);
}
```
---
<div id="glsl_editor"></div>

<script type="text/javascript">
    const glslEditor = new GlslEditor('#glsl_editor', { 
        canvas_size: 500,
        canvas_draggable: true,
        theme: 'monokai',
        multipleBuffers: true,
        watchHash: true,
        fileDrops: true,
        menu: true
    });
</script>
