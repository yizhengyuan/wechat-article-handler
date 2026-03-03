# 📎 WeChat Article Handler for OpenClaw

<p align="center">
  <img src="https://img.shields.io/badge/OpenClaw-Skill-blue?style=flat-square" alt="OpenClaw Skill">
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License">
  <img src="https://img.shields.io/badge/Platform-macOS%20%7C%20Linux-orange?style=flat-square" alt="Platform">
</p>

> 🤖 一键抓取微信文章 → AI 提取精华 → 自动保存到笔记系统

---

## ✨ 效果演示

```
你: https://mp.weixin.qq.com/s/xxxxx

OpenClaw:
┌─────────────────────────────────────────┐
│  📥 正在抓取文章...                      │
│  ✅ 已获取《AI 时代的个人知识管理》       │
│                                          │
│  📝 提取关键信息:                        │
│     • 核心观点 (3条)                     │
│     • 启发收获 (4点)                     │
│     • 行动清单 (2项)                     │
│                                          │
│  💾 已保存至:                            │
│  ~/obsidian-notes/daily/2026-03-04/      │
│  └── reading-list.md                     │
└─────────────────────────────────────────┘
```

---

## 🔄 工作流程

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  📱 发送    │────▶│  🌐 浏览器  │────▶│  🧠 AI分析  │
│  微信链接   │     │  抓取内容   │     │  提取精华   │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
                         ┌─────────────────────▼──────┐
                         │      💾 保存到笔记         │
                         │  daily/YYYY-MM-DD/         │
                         │  └── reading-list.md       │
                         └────────────────────────────┘
```

### 详细步骤

| 步骤 | 动作 | 工具 |
|:---:|:---|:---|
| 1️⃣ | **接收链接** | OpenClaw Message Handler |
| 2️⃣ | **浏览器抓取** | `agent-browser` CLI |
| 3️⃣ | **等待渲染** | `wait --load networkidle` |
| 4️⃣ | **提取结构** | Accessibility Tree Snapshot |
| 5️⃣ | **AI 分析** | LLM (Kimi/Claude/GPT) |
| 6️⃣ | **格式化输出** | Markdown Template |
| 7️⃣ | **追加保存** | File System |

---

## 📝 输出格式

每篇文章自动整理为以下结构：

```markdown
### [文章标题]
- **来源**: 微信公众号 / [公众号名称]
- **作者**: [作者名]
- **链接**: <URL>
- **日期**: YYYY-MM-DD

#### 📋 概述
[2-3句话概括核心论点]

#### 💡 启发
1. [具体收获 1]
2. [具体收获 2]
3. [具体收获 3]
...

#### ✅ 行动
- [ ] [可执行的待办事项 1]
- [ ] [可执行的待办事项 2]
...

**标签**: #tag1 #tag2 #tag3
```

---

## 🚀 快速开始

### 前置要求

- [OpenClaw](https://github.com/elviskahoro/openclaw) 已安装
- [agent-browser](https://www.npmjs.com/package/agent-browser) CLI 工具
  ```bash
  npm install -g agent-browser
  agent-browser install  # 首次使用需下载 Chromium
  ```

### 安装 Skill

```bash
# 进入 OpenClaw skills 目录
cd ~/.openclaw/workspace/skills

# 克隆本仓库
git clone https://github.com/yizhengyuan/wechat-article-handler.git

# 重启 OpenClaw Gateway（使 Skill 生效）
systemctl --user restart openclaw-gateway
```

### 配置保存路径（可选）

默认保存到 `~/obsidian-notes/daily/YYYY-MM-DD/reading-list.md`

自定义路径：

```bash
# 添加到 ~/.zshrc 或 ~/.bash_profile
export WECHAT_ARTICLE_SAVE_DIR="/path/to/your/notes"
export WECHAT_ARTICLE_FILENAME="my-reading-list.md"
```

---

## 🎯 使用示例

### 场景 1：日常阅读收藏

```
你: https://mp.weixin.qq.com/s/_9ERlECVFOd4CsTDXvmIfw

OpenClaw: 
搞定！✅ 已保存到 ~/obsidian-notes/daily/2026-03-04/reading-list.md

📄 《别把 OpenClaw 当 ChatGPT 用了》
   作者: Ben | 来源: AI工具进化论
   
💡 核心启发:
   • OpenClaw 多 Agent 能力远超单聊模式
   • 每个 Agent 有独立工作目录和记忆
   • 群聊绑定实现无需@的自动响应
   
🏷️ 标签: #OpenClaw #多Agent #工作流优化
```

### 场景 2：研究资料整理

连续发送多个链接，自动按日期归类：

```
daily/
├── 2026-03-04/
│   └── reading-list.md  ← 包含当天所有文章
├── 2026-03-03/
│   └── reading-list.md
└── 2026-03-02/
    └── reading-list.md
```

---

## 🔧 高级配置

### 自定义模板

编辑 `SKILL.md` 中的输出格式部分，修改：
- 字段顺序
- 添加自定义字段
- 调整标签策略

### 与 Obsidian 集成

推荐配合以下 Obsidian 插件使用：

| 插件 | 用途 |
|:---|:---|
| Dataview | 按标签筛选文章 |
| Periodic Notes | 自动生成每日笔记 |
| Readwise Official | 同步到其他阅读平台 |

---

## 📊 为什么需要这个？

| 痛点 | 解决方案 |
|:---|:---|
| ❌ 微信文章收藏后吃灰 | ✅ AI 自动提取精华，结构化保存 |
| ❌ 手动复制粘贴费时 | ✅ 一键发送链接，全自动处理 |
| ❌ 收藏多了找不到 | ✅ 按日期归档 + 标签分类 |
| ❌ 看完就忘 | ✅ "启发→行动"强制输出 |

---

## 🤝 贡献

欢迎提交 Issue 和 PR！

想法：
- [ ] 支持更多文章平台（知乎、简书等）
- [ ] 自动生成思维导图
- [ ] 与 Notion/飞书文档集成
- [ ] 阅读统计 Dashboard

---

## 📄 License

MIT © [yizhengyuan](https://github.com/yizhengyuan)

---

<p align="center">
  Made with ❤️ for OpenClaw users
</p>
