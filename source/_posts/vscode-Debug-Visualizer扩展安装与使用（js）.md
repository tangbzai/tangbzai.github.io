---
title: vscode Debug Visualizer扩展安装与使用（js）
date: 2020-06-18 22:56:03
tags: vscode
---
## vscode Debug Visualizer扩展安装与使用（js）

### 安装Debug Visualizer
1. 拓展搜索Debug Visualizer
2. 点击安装即可

### 使用Debug Visualizer
1. 在vscode内按`ctrl`+`shift`+`p`叫出命令输入窗
2. 在命令输入框输入 `Open a new Debug Visualizer View`打开展示台
3. 在展示台上输入 表达式
4. 在编辑器上设置断点
5. 按F5调试


### 表达式

```
hedietDbgVis.markedGrid(
    array,
    hedietDbgVis.tryEval(["i","j","left","right"])
)
```
变量|说明
---|---
`array`|数组变量
`i` `j` `left` `right`|指针