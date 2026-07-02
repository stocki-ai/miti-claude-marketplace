# miti-claude-marketplace

Claude Code plugin marketplace for [Miti](https://www.miti.chat) Agent integrations.

| 项 | 值 |
|----|-----|
| Marketplace 标识 | `miti-claude-marketplace` |
| 插件名 | `miti` |
| npm 包 | [@catpi-miti/miti-claude-code-channel](https://www.npmjs.com/package/@catpi-miti/miti-claude-code-channel) |
| 当前插件版本 | **0.1.1** |

## 前置条件

- Claude Code **≥ 2.1.80**（Channels research preview）
- Node.js **≥ 22**
- Miti Agent 的 **App ID**、**App Secret**（生产环境创建）

## 安装

```bash
claude plugin marketplace add stocki-ai/miti-claude-marketplace
claude plugin install miti@miti-claude-marketplace
```

自检：

```bash
claude plugin marketplace list
claude plugin list
```

## 配置凭据

**macOS / Linux：**

```bash
mkdir -p ~/.claude/channels/miti
cat > ~/.claude/channels/miti/credentials.json <<'EOF'
{
  "appId": "your_app_id",
  "appSecret": "your_app_secret",
  "apiBaseUrl": "https://www.miti.chat/chat"
}
EOF
```

**Windows（PowerShell）：**

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\channels\miti"
@'
{
  "appId": "your_app_id",
  "appSecret": "your_app_secret",
  "apiBaseUrl": "https://www.miti.chat/chat"
}
'@ | Set-Content -Path "$env:USERPROFILE\.claude\channels\miti\credentials.json" -Encoding utf8NoBOM
```

`apiBaseUrl` 可省略，默认 `https://www.miti.chat/chat`。

也可使用环境变量 `MITI_APP_ID`、`MITI_APP_SECRET`、`MITI_API_BASE_URL`（优先级高于凭据文件）。

## 启动 Claude session

**macOS / Linux：**

```bash
cd /path/to/your-project
claude --debug-file /tmp/miti-claude-debug.log \
  --dangerously-skip-permissions \
  --dangerously-load-development-channels plugin:miti@miti-claude-marketplace
```

**Windows（PowerShell）：**

```powershell
cd C:\your-project
claude --debug-file "C:\temp\miti-claude-debug.log" `
  --dangerously-skip-permissions `
  --dangerously-load-development-channels plugin:miti@miti-claude-marketplace
```

> **研究预览期必须**带 `--dangerously-load-development-channels`。自定义 Miti channel 尚未进入 Anthropic 官方 channel allowlist。

## 配对与使用

1. 会话内输入 `/mcp`，确认 `plugin:miti:miti` 为 **✓ Connected**。
2. **单聊**：在 Miti 向 Agent 发送 `pair`，在本机**系统终端**执行：

   ```bash
   npx miti-claude-channel-pair <6位码>
   ```

3. **群 @**：默认无需本地 pairing，直接 @ Agent 即可。

## 升级

```bash
claude plugin marketplace update miti-claude-marketplace
claude plugin install miti@miti-claude-marketplace
```

关闭当前 Claude session 后重新启动。

## 故障排查（简要）

| 现象 | 建议 |
|------|------|
| MCP 未 Connected | 确认已 `plugin install`；用 `--debug-file` 重启 session |
| 收不到消息 | 启动参数须含 `--dangerously-load-development-channels`；单聊须先 pairing |
| 网关连接失败 | 检查 `credentials.json` 中 App ID/Secret 与 `apiBaseUrl` 是否与 Agent 环境一致 |
| 配对无效 | 在系统终端执行配对命令，不要在 Claude 对话框输入 |

插件日志前缀为 `[miti-channel]`，输出在运行 `claude` 的终端 stderr。

**备选安装：** 若 marketplace 不可用，可从 npm 直装 `@catpi-miti/miti-claude-code-channel`，启动时使用 `--plugin-dir` 与 `plugin:miti@inline`。详见 [npm 包 README](https://www.npmjs.com/package/@catpi-miti/miti-claude-code-channel)。

## 维护者

本仓库仅包含 marketplace 清单；插件实现发布在 npm。

版本维护规则：

- **插件发新版**：先在 npm 发布 `@catpi-miti/miti-claude-code-channel`，再更新 `.claude-plugin/marketplace.json` 中 `plugins[0].source.version`。
- **只改 marketplace 清单**（描述、结构等）：可只 bump `metadata.version`，不必改 npm pin。
- 不在 `plugins[0].version` 重复声明插件版本；以 npm 包内 `.claude-plugin/plugin.json` 为准。

提交前校验：

```bash
claude plugin validate .
```
