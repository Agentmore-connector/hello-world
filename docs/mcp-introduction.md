# MCP（Model Context Protocol）简介

## 什么是 MCP

MCP（Model Context Protocol，模型上下文协议）是由 Anthropic 于 2024 年 11 月推出的一项开放协议，旨在为 AI 应用（如大语言模型客户端）与外部数据源、工具和服务之间建立标准化的连接方式。

可以把它类比为「AI 应用的 USB-C 接口」：不管底层是什么模型、外部是什么服务，只要双方都遵循 MCP，就能即插即用地完成对接。

## 核心价值

- **标准化**：统一了 AI 与外部系统的集成方式，避免为每个数据源单独开发定制连接器。
- **开放生态**：任何人都可以实现 MCP 服务器或客户端，形成可复用的工具生态。
- **解耦**：模型、AI 应用与数据源三者解耦，任意一方变化不影响其他方。
- **安全性**：内置对数据访问控制和用户授权的考量，工具能力可按需暴露。

## 架构组成

MCP 采用客户端-服务器（Client-Server）架构：

| 角色 | 说明 |
| --- | --- |
| **MCP Host** | 运行 AI 应用的宿主程序，如 IDE、聊天客户端、Agent 框架 |
| **MCP Client** | 宿主内部的连接器，负责与 MCP Server 保持 1:1 通信 |
| **MCP Server** | 暴露具体能力的轻量服务，如访问数据库、调用 API、读取文件 |

## 三大核心能力

1. **Resources（资源）**：向模型暴露上下文数据，如文件内容、数据库记录、API 响应等，由应用侧控制。
2. **Tools（工具）**：让模型可以主动调用的操作，如创建 Issue、执行查询、发送消息，由模型控制。
3. **Prompts（提示模板）**：预置的可复用提示词模板，由用户选择触发。

## 典型应用场景

- **开发工具集成**：让 AI 编程助手直接读写 GitHub 仓库、提交 PR、管理 Issue。
- **企业数据接入**：连接内部知识库、CRM、数据库，让 AI 回答基于真实业务数据。
- **自动化工作流**：结合 Agent，实现「一句话完成跨系统操作」的自动化流程。

## 快速上手

以 TypeScript SDK 为例，创建一个最简单的 MCP Server：

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new McpServer({ name: "demo-server", version: "1.0.0" });

// 注册一个简单工具
server.tool("hello", { name: { type: "string" } }, async ({ name }) => ({
  content: [{ type: "text", text: `Hello, ${name}!` }],
}));

await server.connect(new StdioServerTransport());
```

常用资源：

- 官网与规范：https://modelcontextprotocol.io
- TypeScript SDK：https://github.com/modelcontextprotocol/typescript-sdk
- Python SDK：https://github.com/modelcontextprotocol/python-sdk
- 示例服务器合集：https://github.com/modelcontextprotocol/servers

## 小结

MCP 正在成为 AI 应用连接现实世界的事实标准。掌握 MCP，意味着你可以让任何模型安全、高效地使用你手上的数据与工具。
