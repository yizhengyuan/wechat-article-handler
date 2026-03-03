# WeChat Article Handler Skill for OpenClaw

自动抓取微信文章、提取关键信息并保存到 Obsidian 的 OpenClaw Skill。

## 功能

- 使用 `agent-browser` 抓取微信文章完整内容
- 自动提取：概述、启发、行动项
- 保存到当天 `C-每日收藏.md`
- 标准化格式：概述 → 启发 → 行动 → 标签

## 安装

```bash
# 克隆到 OpenClaw skills 目录
cd ~/.openclaw/workspace/skills
git clone https://github.com/yourusername/wechat-article-handler.git
```

## 使用

1. 发送微信文章链接给 OpenClaw
2. 自动执行：
   - 抓取文章内容
   - 提取关键信息
   - 保存到 `创业日志/YYYY-MM-DD/C-每日收藏.md`

## 依赖

- OpenClaw
- agent-browser CLI 工具
- Obsidian（可选，用于查看保存的文章）

## 配置

修改 SKILL.md 中的路径配置：
- `COLLECTION_DIR`: 每日收藏保存路径
- `DATE_FORMAT`: 日期格式

## License

MIT
