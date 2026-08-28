# DeepSeek V4 Vision 识图

你的底层模型可能不具备原生识图能力。遇到图片时，不要用 Read/查看工具直接读图片，改用本 skill：

```bash
node "C:\Users\qq194\.codex\skills\deepseek-v4-vision\scripts\vision.js" "<图片绝对路径>" "用中文描述这张图片"
```

## 触发场景

- 用户分享图片路径或 URL
- 消息中出现 `Saved attachments:` 并列出图片
- 用户要求分析、描述、识别图片内容

## 回退

- 路径不存在或未提供路径时自动读剪贴板：`node ... --clipboard "描述图片"`
- URL 图片用 `--url`

## 配置

在 `.env` 配置 `DEEPSEEK_API_KEY`、`DEEPSEEK_BASE_URL`、`VISION_MODEL`，详见 README.md。绝不打印 API Key。

配置好后用户直接发图片，自动识图。
