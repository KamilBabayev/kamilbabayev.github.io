---
layout: post
title:  "Spring Animator"
categories: JavaScript
tags: React hooks
author: Aastha Mehta
mathjax: true
---

* content
{:toc}

INFO for blog --3 liner




{% raw %}
## Heading

### `timing-function` sub -heading 

Rest text plain `timing-function-highlight` ended highlight here.

<a href="https://cubic-bezier.com/">
  <img width="70%" src="https://gw.alicdn.com/imgextra/i4/O1CN01rCo6Er1jz5h57YNjg_!!6000000004618-2-tps-978-352.png"/>
</a>

**Strong highlight**。Baaju main sentence shuru。

### 弹簧-阻尼系统中的运动

Native rendering below

![Image not found](https://gw.alicdn.com/imgextra/i4/O1CN01r7hDZ81Uulic8TukO_!!6000000002578-2-tps-216-139.png)

Simple line：

$$
F_s=-kX  // this is a formula interpreted
$$

One more formula
$$
\begin{cases}
  F=-kX-cv \\
  F=ma
\end{cases}
$$

$$
\begin{cases}
  f(0) = 0 \\
  f'(0) = 0 \\
  f''(t) = -180(f(t) - 1) - 12f'(t)
\end{cases}
$$

 [Google Link](https://www.google.com) 

```
f(0) = 0; f'(0) = 0; f''(t) = -180(f(t) - 1) - 12f'(t)   # Black Box highlight
```

Heading

![Pic Not Found Msg](https:)



### SHM

SHM Info in grey box below：

> Special Grey highlight box /// Quote Box。

A bulleted list below

- (zero damping)：
- (light damping)
- (critical damping)：
- (over damping)：

## Table Below with video & image side by side

<table>
<tr>
  <td>
    <video src="https://g.alicdn.com/ltao-fe/assets/damping.mp4" autoplay controls preload loop muted width="300px"></video>
  </td>
  <td>
    <a href="https://gaohaoyang.github.io/framer-motion-practice/#/Spring">Image below</a> <br />
    <img src="https://gw.alicdn.com/imgextra/i2/O1CN011yInSb1UIIRunzpdL_!!6000000002494-2-tps-200-200.png"/>
  </td>
</tr>
</table>

Table

name | download values
--- | ---
react-motion | <img src="https://gw.alicdn.com/imgextra/i1/O1CN01jwQeCd21tHkxwg9h1_!!6000000007042-2-tps-528-110.png" width="300px"/>
react-spring | <img src="https://gw.alicdn.com/imgextra/i1/O1CN01EZY4kL1zfswDwcpgZ_!!6000000006742-2-tps-510-114.png" width="300px"/>
framer-motion | <img src="https://gw.alicdn.com/imgextra/i4/O1CN01RsV09s1abQgessyq0_!!6000000003348-2-tps-526-114.png" width="300px"/>

Spring demo

```jsx
import React from 'react'
import { motion } from 'framer-motion'

import './index.css'

function index() {
  return (
    <>
    <div className="wrap">
       Coding Block 
    </div>
  )
}

export default index
```

demo CSS

```jsx
animate={{
  x: 150,
  transition: {
    type: 'spring', //comment placed here
    damping: 0, 
  },
}}
```

Video

<video src="https://g.alicdn.com/ltao-fe/assets/damping_render.mp4" autoplay controls preload loop muted width="100%"></video>


## Sum UP

Type Summary here

Meet u again!

## PDF Version Preview

<object data="https://drive.google.com/file/d/1ZKgZhUuBr_-nLnbpExhQmqKCx_1QqxT2/preview"  width="80%" height=500> <embed src="https://drive.google.com/file/d/1ZKgZhUuBr_-nLnbpExhQmqKCx_1QqxT2/preview" width="600px" height="500px" /> <p>This browser does not support PDFs. Please download the PDF to view it: <a href="https://drive.google.com/file/d/1ZKgZhUuBr_-nLnbpExhQmqKCx_1QqxT2/preview">View the PDF</a>.</p> </embed></object>

{% endraw %}
