---
name: wechat-article-handler
description: 处理微信文章链接 - 抓取内容、提取关键信息、保存到Obsidian每日收藏
metadata:
  openclaw:
    emoji: 📎
---

# WeChat Article Handler - 微信文章处理

自动处理衣哥发来的微信文章链接，完成抓取→提取→保存的完整流程。

## 触发条件

用户发送以 `https://mp.weixin.qq.com/s/` 开头的链接

## 处理流程

```
收到微信文章链接
    ↓
1. agent-browser 抓取完整内容
   - open URL → wait networkidle → snapshot
    ↓
2. 提取关键信息
   - 标题、作者、来源公众号
   - 发布日期
   - 核心观点（3-5条）
   - 启发/收获
   - 可执行的行动项
    ↓
3. 按标准格式保存到 Obsidian
   - 路径：创业日志/YYYY-MM-DD/C-每日收藏.md
   - 格式：概述 → 启发 → 行动 + 标签
    ↓
4. 返回确认
   - 告知保存位置
   - 简要总结文章内容
```

## 输出格式模板

```markdown
### [文章标题]
- **来源**：微信公众号 / [公众号名称]
- **作者**：[作者名]（如有）
- **链接**：<URL>

#### 概述
[2-3句话概括核心内容]

#### 启发
1. [具体收获1]
2. [具体收获2]
3. [具体收获3]
...

#### 行动
- [ ] [待办事项1]
- [ ] [待办事项2]
...

**标签**：[#标签1 #标签2 ...]
```

## 使用示例

**输入**：
```
https://mp.weixin.qq.com/s/_9ERlECVFOd4CsTDXvmIfw
```

**处理**：
1. 执行：`agent-browser open <url> && agent-browser wait --load networkidle && agent-browser snapshot`
2. 解析返回的 accessibility tree
3. 提取标题、作者、正文要点
4. 生成概述/启发/行动
5. 追加到当日 C-每日收藏.md

**输出确认**：
```
搞定！✅ 已保存到 创业日志/2026-03-04/C-每日收藏.md

**文章标题**：别把 OpenClaw 当 ChatGPT 用了...
**核心内容**：OpenClaw 多 Agent 配置教程...
**标签**：#OpenClaw #多Agent #AI工具
```

## 注意事项

1. **agent-browser 依赖**：确保已安装 `agent-browser` CLI 工具
2. **网络等待**：微信文章需等待 JavaScript 渲染，必须使用 `wait --load networkidle`
3. **内容截断**：如果 snapshot 返回内容不完整，尝试增加等待时间或重新抓取
4. **重复检测**：同一链接不要重复保存，检查当日收藏是否已存在
5. **分类规则**：
   - "我的输出" = 衣哥自己写的文章
   - "每日收藏" = 外部阅读的文章（默认）

## 相关文件

- 保存目标：`~/Desktop/yzy-docs/创业日志/YYYY-MM-DD/C-每日收藏.md`
- 历史收藏：`~/Desktop/yzy-docs/文档汇总/每日收藏合集/`
