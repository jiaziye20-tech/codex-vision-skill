---
name: claude-vision-skill
description: 让无原生识图能力的模型获得识图能力——把图片发给有 vision 的模型，用文字描述回来。使用 node vision.js 调用阿里云百炼（千问 VL）/ OpenAI 等 vision 模型 API，支持本地图片和 URL。
metadata:
  short-description: 通过 vision API 为无识图能力的模型提供图片分析
---

# Vision Skill

你的底层模型不具备原生识图能力。遇到图片时，使用 
ode vision.js 调用外部 vision 模型进行识别。

## 触发场景

- 用户分享图片路径（本地或网络 URL）
- 消息中出现 "Saved attachments:" 并列出图片
- 用户要求分析、描述、识别图片内容

## 使用方法

`ash
node vision.js "<图片路径>" "用中文描述这张图片"
node vision.js --url "<图片链接>" "图片里有什么？"
`

## 识图服务

默认使用阿里云百炼千问 VL 模型（推荐：新用户 100 万 token 免费）。

| 服务 | 模型 | 备注 |
|------|------|------|
| **阿里云百炼（推荐）** | qwen3.5-omni-plus / qwen-vl-max | 新用户免费 |
| OpenAI | gpt-4o-mini | 需海外支付 |
| 其他 | 任何 OpenAI 兼容格式 | 改 BASE_URL 和模型名 |

## 配置

首次使用前需配置 API Key：

1. 设置环境变量 DASHSCOPE_API_KEY（阿里云百炼）或修改 ision.js 中的 API_KEY
2. 如果用非千问服务，同时修改 BASE_URL 和 MODEL
3. 获取 Key: https://bailian.console.aliyun.com/

## 依赖

`ash
npm install dotenv
`

## 工作原理

ision.js 是核心脚本，读取图片 → base64 编码 → 发送给 vision 模型 API → 返回文字描述。走 OpenAI 兼容格式，不绑定特定厂商。

---

## 如果你是 AI（Codex），请读这里

用户把这个 skill 安装好，是想让你帮他配置识图。先确认信息，再按场景执行。

### 先确认

问用户：
1. 想用哪个识图服务？（推荐千问 qwen3.5-omni-plus）
2. API Key 是什么？没申请的话引导去 https://bailian.console.aliyun.com/ 注册
3. （如果用非千问服务）API Base URL 是什么？

### 场景 A：用户要在普通项目里用

1. 把 ision.js 复制到项目根目录
2. 替换 ision.js 里的 sk-xxx → 用户的 Key，xxx → 模型名。如果用的不是千问，同步改 BASE_URL
3. 告诉用户：配置完成，以后直接发图片就行，Codex 会自动识图

### 场景 B：用户只想了解这是什么

简要解释：
- ision.js 是核心，读取图片 → base64 → 发给 vision 模型 API → 返回文字描述
- 走 OpenAI 兼容格式，不绑定特定厂商
- 推荐千问是因为有免费额度
- 配置好之后，用户直接发图片，AI 自动处理
