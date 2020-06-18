---
title: "CSS动画（animation与keyframes）"
date: 2019-04-02 01:05:00
tags:
    - CSS
---
## animation
```css
animation: name duration timing-function delay iteration-count direction;
```
属性名 | 中文名 | 值
:-------------------------|:-------------------|:-------------------:
animation-name            | keyframe名称       | /
animation-duration        | 持续时间           | (time)
animation-timing-function | 速度曲线           | linear &#124; ease &#124; ease-in &#124; ease-out &#124; <br> ease-in-out &#124; cubic-bezier(n,n,n,n)
animation-delay           | 开始前的延迟       | (time)
animation-iteration-count | 播放次数           | (n) &#124; infinite
animation-direction       | 是否轮流反向播放    | normal &#124; alternate
animation-play-state      | 指定动画播放或暂停  | paused &#124; running
注：
1. time为时间值默认毫秒为单位，可设置秒为单位（ns）
2. n表示任意整数

## @keyframes
```css
@keyframes name {
    from {
        color: #fff;
    }
    to {
        color: #000;
    }
}
```
```css
@keyframes name {
    0% {
        color: #fff;
    }
    100% {
        color: #000;
    }
}
```
- 上面两种效果是等同的<br>
（可用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"）
- 0% 是动画的开始，100% 是动画的完成。
- 避免兼容性问题，应该始终定义 0% 和 100% 选择器