# Matriangle Vue UI

![GitHub License](https://img.shields.io/github/license/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge&color=78dce8)
![Code Size](https://img.shields.io/github/languages/code-size/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge&color=78dce8)
![Lines of Code](https://www.aschey.tech/tokei/github.com/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge&color=78dce8)
[![Language](https://img.shields.io/badge/language-TypeScript-cyan?style=for-the-badge&color=78dce8)](https://www.typescriptlang.org/)

![Created At](https://img.shields.io/github/created-at/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge)
![Last Commit](https://img.shields.io/github/last-commit/ARCJ137442/Matriangle-Vue-UI?style=for-the-badge)

> ⚠️ **项目状态：停止维护（2026-06-29 更新）**
>
> 本项目已许久未维护，后续很长一段时间预期不再维护。
>
> - 🔒 仓库 watch 已禁用，**不再发送 Dependabot 告警邮件**
> - ✅ 2026-06-29 已一次性升级 `webpack-dev-server` 4→5.2.5、`html-webpack-plugin` 5.5.3→5.6.7，并通过 `overrides` 锁住 11 个传递依赖到修复版本
> - ✅ Dependabot 扫描状态：30/30 漏洞已 `fixed`（1 critical + 14 high + 14 medium + 1 low）
> - ⚠️ 不推荐 fork 后继续开发；如需参考实现，可直接克隆后修改

## 🤖 Agent 维护披露

> 本仓库自 2026-06-29 起的维护性 commit（包括本次依赖升级、Dependabot 告警处理、文档同步）均由 AI Agent **MiniMax-M3** 在用户监督下协助完成。
>
> - **Agent 角色**：执行批量依赖升级、npm audit 验证、配置文件迁移（webpack-dev-server 4→5）、lock 文件重生成
> - **人工监督**：所有 commit 由用户审阅后手动 push
> - **披露原则**：遵循「Agent 协作必须可追溯」原则，commit 信息、文档修改、依赖变更均可被审计
> - **能力边界**：本项目不再接受新功能开发，仅做安全维护；如发现新漏洞请自行处理

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
