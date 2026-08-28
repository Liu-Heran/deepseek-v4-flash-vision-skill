---
name: deepseek-v4-vision
description: 当用户发送、粘贴、拖入、附加或引用图片（本地路径、URL、剪贴板、Saved attachments），需要描述、分析、识别、OCR、图表解析或视觉 Agent 任务，且当前模型不具备原生识图能力时使用。运行本 skill 的 scripts/vision.js，调用 DeepSeek V4 Flash Vision（deepseek-v4-flash-vision-exp）把图片转成文字。
---

# DeepSeek-V4 Vision 视觉调用

当前模型没有原生视觉输入时，不要尝试直接用查看工具“看”图片，统一运行本 skill 的 `scripts/vision.js`，把图片发送给 DeepSeek V4 Flash Vision 并拿回文字结果。

## 用法

本机已安装路径（Codex skills 目录）：

```bash
node "C:\Users\qq194\.codex\skills\deepseek-v4-vision\scripts\vision.js" "<图片绝对路径>" "用中文描述这张图片"
node "C:\Users\qq194\.codex\skills\deepseek-v4-vision\scripts\vision.js" --url "<图片URL>" "分析这张图片"
node "C:\Users\qq194\.codex\skills\deepseek-v4-vision\scripts\vision.js" --clipboard "识别剪贴板里的文字"
```

如果该 skill 被复制到项目中使用，用项目内的实际相对路径，例如 `scripts/vision.js`。

## 触发场景

- 用户分享本地图片路径或网络图片 URL
- 消息中出现 `Saved attachments:` 并列出图片
- 用户要求描述、分析、识别、OCR、图表解析图片
- 视觉 Agent 任务需要把图片内容转为文字后继续处理

## 回退规则

- 本地路径不存在，或没有提供路径/URL 时，`vision.js` 自动读取系统剪贴板
- 传 `--no-fallback` 可关闭自动回退，改为明确报错
- `--clipboard` 不能与图片路径或 `--url` 同时使用

## 配置

配置优先级：环境变量 > `scripts/.env`。也可直接放脚本同目录 `.env`。

```env
DEEPSEEK_API_KEY=sk-你的Key
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
VISION_MODEL=deepseek-v4-flash-vision-exp
VISION_MAX_TOKENS=1024
```

完整配置说明和 DeepSeek API 申请教程见 [README.md](README.md)。

## 规则

- 必须使用脚本绝对路径或项目内确定路径
- 图片用绝对路径，URL 用 `--url`，粘贴的图片优先 `--clipboard`
- 默认用中文描述，除非用户另有要求
- 多张图片必须逐张处理，拿到全部描述后再回复
- 绝不打印、分享或提交 API Key
- API 调用失败时，如实报告错误，并请用户检查 Key、模型名或 Base URL
