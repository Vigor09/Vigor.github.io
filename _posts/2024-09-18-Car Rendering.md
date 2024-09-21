---
title: 汽车渲染心得
date: 2024-09-18 20:11:00 +0800
categories: [Graphics,Shader]
tags: [graphics,glsl]
---

*整合一下项目的汽车渲染需求*

---

### 前言
*我目前的工作是新能源的智能座舱中3D HMI部分，其中3D车模的渲染效果是重点中的重点。  
但是基础的PBR并不满足实时渲染上汽车的渲染需求，所以打算总结一下项目的经验，梳理一个好的汽车展示中，渲染效果到底需要什么。*

---

### 汽车渲染需要展示什么？

- 结构
我们在拿到一个车模时，因为基本上拿到的数模都是从UG或者PDPS中提取的曲面或JT格式，有着成千上万的面。  
所以首先需要给美术的同事进行轻量化重新布线的工作，这个环节非常重要。通常我们是按材质去将一辆车的结构拆分为 

1.***CarPaint***（车漆）  
2.***Tire***（胎皮）  
3.***Wheel***（轮毂）  
4.***Window***（车窗）  
5.***Lamp***（车灯）  
6.***Others***（底盘侧裙，ABC柱等与车漆不同材质的部分）  
7.***Chrome***（镀铬件）  
8.***Interior***（所有内饰）  

以小米su7为例：  
![Mesh节点](/assets/img/postAssets/su7.jpg)   

>PS：曾经Tire、Wheel、Chrome都是合在Others里面，用ARM贴图区分。但是设计师在调整材质的，金属和粗糙值会影响全局导致会全局调整，所以还是拆分出来了。

---
#### CarPaint

- 车漆组成 
  - 底漆层 - Base Color Layer 
  - 清漆层 - Clear Coat Layer
  - 珠光层 - Flake Layer

***vertex shader***
```glsl
attribute vec3 kzPosition;
attribute vec2 kzTextureCoordinate0;
uniform highp mat4 kzProjectionCameraWorldMatrix;
uniform mediump vec2 TextureOffset;
uniform mediump vec2 TextureTiling;

varying mediump vec2 vTexCoord;

void main()
{
    precision mediump float;

    vTexCoord = kzTextureCoordinate0*TextureTiling + TextureOffset;
    gl_Position = kzProjectionCameraWorldMatrix * vec4(kzPosition.xyz, 1.0);
}
```
***fragment shader***
```glsl
uniform sampler2D Texture;
uniform lowp float BlendIntensity;
varying mediump vec2 vTexCoord;


void main()
{
    precision lowp float;

    vec4 color = texture2D(Texture, vTexCoord);
    gl_FragColor.rgba = color.rgba * BlendIntensity;
}
```
---
持续更新
