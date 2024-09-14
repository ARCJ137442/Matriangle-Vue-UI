# Matriangle Vue UI

![GitHub License](https://img.shields.io/github/license/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge&color=78dce8)
![Code Size](https://img.shields.io/github/languages/code-size/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge&color=78dce8)
![Lines of Code](https://www.aschey.tech/tokei/github.com/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge&color=78dce8)
[![Language](https://img.shields.io/badge/language-TypeScript-cyan?style=for-the-badge&color=78dce8)](https://www.typescriptlang.org/)

![Created At](https://img.shields.io/github/created-at/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge)
![Last Commit](https://img.shields.io/github/last-commit/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge)

## Overview 概述

基于Vue+TypeScript+Webpack的Matriangle客户端

- 🎯适用于展示Matriangle的各个实例
- 📌【2023-12-03 23:02:31】目前刚从Matriangle分离独立

## Quick Start 快速开始

### 安装

作为一个npm包，其可通过`git clone`被下载，并通过npm进行部署：

```bash
git clone https://github.com/ARCJ137442/Matriangle-Vue-UI.git
npm install
```

### 使用

#### 构建「基于Node服务端」的客户端

```bash
npm run build-n
```

构建「与Node服务端进行连接」并且「使用Websocket进行通信」的客户端（不把服务端集成进客户端中）

#### 构建「纯浏览器」的客户端（集成NARS测试环境）

```bash
npm run build-bn
```

构建「不依赖Node，逻辑显示一同在浏览器端运作」且「上载了NARS测试环境」的一体式客户端

#### 调试「基于Node服务端」的客户端

```bash
npm run dev-n
```

调试「与Node服务端进行连接」并且「使用Websocket进行通信」的客户端（不把服务端集成进客户端中）

#### 调试「纯浏览器」的客户端（集成NARS测试环境）

```bash
npm run dev-bn
```

调试「不依赖Node，逻辑显示一同在浏览器端运作」且「上载了NARS测试环境」的一体式客户端

## 许可证

包的**整体**以 ***Lesser GNU General Public License v3(LGPLv3)*** 协议发布

- 使用者可以在**不修改源码时**私用
- 若使用者修改了源码，则必须将源码以相同协议发布
