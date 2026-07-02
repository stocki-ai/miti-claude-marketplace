# miti-claude-marketplace

Claude Code plugin marketplace for [Miti](https://www.miti.chat) Agent integrations.

| 项 | 值 |
|----|-----|
| GitHub | [stocki-ai/miti-claude-marketplace](https://github.com/stocki-ai/miti-claude-marketplace) |
| Marketplace 标识 | `miti-claude-marketplace` |
| 插件 | `miti` → npm `@catpi-miti/miti-claude-code-channel` |

完整用户安装与故障排查见 [miti-agent-docs/claude-code-user-install.md](../miti-agent-docs/claude-code-user-install.md)（monorepo 内）或团队文档站。

## 用户安装

```bash
claude plugin marketplace add stocki-ai/miti-claude-marketplace
claude plugin install miti@miti-claude-marketplace
```

配置 `~/.claude/channels/miti/credentials.json` 后启动 session：

```bash
claude --debug-file /tmp/miti-claude-debug.log \
  --dangerously-skip-permissions \
  --dangerously-load-development-channels plugin:miti@miti-claude-marketplace
```

> **研究预览期必须**带 `--dangerously-load-development-channels`。自定义 Miti channel 尚未进入 Anthropic 官方 channel allowlist。

## 维护者

版本维护规则：

- 插件发新版：先发布 npm 包，再只更新 `.claude-plugin/marketplace.json` 中 `plugins[0].source.version`。
- 只改 marketplace 清单、说明或插件列表：只更新 `metadata.version`（可选），不要改 npm pin。
- 不在 `plugins[0].version` 重复声明插件版本；插件自身版本以 npm 包内 `.claude-plugin/plugin.json` 为准。

```bash
claude plugin validate .
```

发版联动见 `miti-claude-code-channel/PUBLISHING.md`。
