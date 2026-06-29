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

> ⚠️ **重要边界说明：以下披露仅适用于「部分最新提交」，不代表项目历史代码由 Agent 编写。**

**历史代码归属**：本仓库 2023-12 至 2024 年的全部历史 commit（包含绝大部分源码、文档、配置）均由项目作者 `ARCJ137442` **人工编写**，AI Agent **未参与**。这些历史 commit 不含 `Co-Authored-By: MiniMax-M3` 标记。

**Agent 维护范围**（**仅限以下 4 个 commit**，2026-06-29）：

| Commit 主题 | 改动内容 | 协作 Agent |
|------------|----------|-----------|
| `docs: 精确化 Agent 维护披露边界` | README 修正披露边界（明确「仅部分最新提交」）| MiniMax-M3 |
| `docs: 标注项目停止维护状态 + Agent 维护披露` | README 顶部加维护状态 + Agent 披露区块 | MiniMax-M3 |
| `fix(deps): bump postcss to 8.5.16` | postcss 8.5.10 → 8.5.16（满足 @vue/compiler-sfc 8.5.15 要求）| MiniMax-M3 |
| `chore(deps): upgrade webpack-dev-server to 5.2.5` | 升级 webpack-dev-server 4→5.2.5、html-webpack-plugin 5.6.7、加 overrides 锁 11 个传递依赖 | MiniMax-M3 |

> 📋 完整 commit 列表请见 [commits 页面](https://github.com/ARCJ137442/Matriangle-Vue-UI/commits/main)（按时间倒序查看 2026-06-29 的所有 commit）

**Agent 在上述 3 个 commit 中的具体工作**：
- 执行 `npm install` 重新生成 lock 文件
- 验证 `npm audit` 与 `npm run build-n` 通过
- 适配 `webpack-dev-server` 4→5 大版本升级的配置语法
- 撰写本次 README 披露区块
- 决定取消仓库 watch 订阅（`PUT /subscription ignored=true`）

**未由 Agent 完成的工作**：
- 业务逻辑代码（Vue 组件、TypeScript 模块、Webpack 配置）
- 项目架构设计
- 依赖选型决策
- 测试用例

**披露原则**：所有 Agent 协助的 commit 在 git 历史中可审计；如需查看具体改动，对照上述 commit hash 即可定位。

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
