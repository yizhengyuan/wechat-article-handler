---
name: wechat-article-handler
description: 处理微信文章链接 - 抓取内容、提取关键信息、保存到指定位置
metadata:
  openclaw:
    emoji: 📎
---

# WeChat Article Handler - 微信文章处理

自动处理微信文章链接，完成抓取→提取→保存的完整流程。

## 配置（用户自定义）

在使用前，请修改以下配置：

```yaml
# config.yaml 示例
save_location: "~/Documents/Obsidian/创业日志"  # 文章保存目录
filename_pattern: "YYYY-MM-DD-daily-collection.md"  # 文件名格式
template_format: "overview-insight-action"  # 输出格式模板
```

或使用环境变量：
```bash
export WECHAT_SAVE_DIR="~/Documents/Obsidian/收藏"
export WECHAT_FILENAME_FORMAT="%Y-%m-%d-readings.md"
```

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
3. 按标准格式保存到指定位置
   - 路径：用户配置的 save_location
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
5. 追加到用户配置的保存位置

**输出确认**：
```
搞定！✅ 已保存到 ~/Documents/Obsidian/收藏/2026-03-04-readings.md

**文章标题**：别把 OpenClaw 当 ChatGPT 用了...
**核心内容**：OpenClaw 多 Agent 配置教程...
**标签**：#OpenClaw #多Agent #AI工具
```

## 注意事项

1. **agent-browser 依赖**：确保已安装 `agent-browser` CLI 工具
2. **网络等待**：微信文章需等待 JavaScript 渲染，必须使用 `wait --load networkidle`
3. **内容截断**：如果 snapshot 返回内容不完整，尝试增加等待时间或重新抓取
4. **重复检测**：同一链接不要重复保存，检查当日文件是否已存在
5. **配置优先顺序**：环境变量 > config.yaml > 默认值

## 默认配置

如果没有自定义配置，使用以下默认值：
- **保存目录**：`~/Documents/wechat-articles`
- **文件名格式**：`%Y-%m-%d-readings.md`
- **模板格式**：概述 → 启发 → 行动

## 相关文件

- Skill 位置：`~/.openclaw/workspace/skills/wechat-article-handler/`
- 配置文件：`config.yaml`（可选）
