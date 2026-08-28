# 让 Agent 调用 DeepSeek 视觉模型

> DeepSeek V4 Flash Vision Skill · 支持 Codex / Claude Code / AGENTS.md 智能体

让没有原生识图能力的模型获得识图能力：通过 `vision.js` 把图片发给 DeepSeek V4 Flash Vision（`deepseek-v4-flash-vision-exp`），再把文字描述拿回来。走 OpenAI 兼容接口，支持本地图片、URL、剪贴板三种输入。

## 目录结构

```text
deepseek-v4-vision/
├── SKILL.md            # Codex skill 入口说明
├── AGENTS.md           # 支持 AGENTS.md 的智能体加载说明
├── CLAUDE.md           # Claude Code 加载说明
├── README.md           # 人类阅读的配置与申请教程
├── scripts/
│   ├── vision.js       # 核心识图脚本
│   ├── clipboard.ps1   # Windows 剪贴板读图
│   └── clipboard.swift # macOS 剪贴板读图
└── agents/openai.yaml  # UI 展示元数据
```

## 配置说明

需要 Node.js（建议 18+）。脚本会在运行目录和脚本同目录查找 `.env`，也可以直接设置环境变量。

在 `scripts/` 下新建 `.env`：

```env
DEEPSEEK_API_KEY=sk-你的DeepSeekKey
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1
VISION_MODEL=deepseek-v4-flash-vision-exp
VISION_MAX_TOKENS=1024
```

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `DEEPSEEK_API_KEY` | 无，必填 | DeepSeek API Key |
| `DEEPSEEK_BASE_URL` | `https://api.deepseek.com/v1` | OpenAI 兼容接口地址 |
| `VISION_MODEL` | `deepseek-v4-flash-vision-exp` | 视觉模型名 |
| `VISION_MAX_TOKENS` | `1024` | 单次最大输出 token 数 |

## 使用示例

本地图片：

```bash
node scripts/vision.js "C:\path\to\image.png" "用中文描述这张图片"
```

网络图片：

```bash
node scripts/vision.js --url "https://example.com/image.jpg" "识别图片中的文字"
```

剪贴板图片：

```bash
node scripts/vision.js --clipboard "分析这张截图"
```

路径不存在或未提供路径时，脚本会自动回退到剪贴板；传 `--no-fallback` 可关闭。

## 多智能体加载

- **Codex / OpenAI Agents**：把本目录放到 `~/.codex/skills/` 下，Codex 通过 `SKILL.md` 自动发现。
- **Claude Code**：把 `CLAUDE.md` 复制到项目根目录，Claude Code 会读取并按规则调用 `vision.js`。
- **支持 AGENTS.md 的智能体**（如 GitHub Copilot 等）：把 `AGENTS.md` 复制到项目根目录。
- **其他智能体**：直接调用 `node scripts/vision.js ...`，或在智能体指令里加入同样的使用规则。

## DeepSeek API 申请简略教程

1. 打开 [DeepSeek 开放平台](https://platform.deepseek.com)，注册并登录账号。
2. 按平台提示完成手机/邮箱验证，如需实名认证按页面要求完成。
3. 进入“充值”页面，给账号充值余额。DeepSeek 是按量付费，余额不足时 API 无法调用。
4. 进入“API Keys”页面，创建一个新的 API Key，复制保存（只显示一次，丢了要重新创建）。
5. 把 Key 填到 `scripts/.env` 的 `DEEPSEEK_API_KEY`，保留 `VISION_MODEL=deepseek-v4-flash-vision-exp`。
6. 运行一次示例命令，能返回文字描述说明配置成功。

## 注意事项

- 目前 `deepseek-v4-flash-vision-exp` 是实验版模型，接口和价格可能与正式版不同。
- 图片会按 token 计费，单张图片最多约 384 tokens，价格与 V4-Flash 一致。
- 支持 JPEG、PNG、GIF、WebP 格式。
- 不要把 `.env` 提交到 git，也不要向任何人展示 API Key。
