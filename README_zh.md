# autoresearch-pro

通过小幅修改、同任务前后对照和保留/回退实验，优化已有 skill、提示词、文章、插件、工作流或系统机制。

[English](README.md) · [核心规则](SKILL.md)

原名为 **cjl-autoresearch-cc**。GitHub 仓库也已更名为 autoresearch-pro，客户端中的目录名、frontmatter 和调用名称统一为 **autoresearch-pro**。

## 使用方式

适合明确要求迭代、用案例比较，或依据失败记录优化既有产物的任务。普通单次润色、翻译、概念解释以及没有实验需求的常规开发不需要调用。

> 使用 autoresearch-pro 优化这段分类提示词。保留原版，用少量实际输入比较前后结果，只做一轮；不要调用付费 API 或修改外部系统。

默认一轮；只有具体问题值得继续才追加，最多三轮。先保留旧版，固定任务，再检查真实产物。没有运行条件时单独交付待验证候选，不用模拟评分宣称改善。不再要求用户选择通用清单或执行长达 100 轮的循环。

## 安装与各 Agent 适配

不需要安装依赖、执行安装脚本或配置模型密钥。先将仓库克隆到稳定的共享目录：

```sh
mkdir -p ~/.agents/skills
git clone https://github.com/0xcjl/autoresearch-pro.git ~/.agents/skills/autoresearch-pro
```

目标必须不存在；已有安装先检查并备份，不能覆盖本地改动。记录 checkout 的 `git rev-parse HEAD`；以后仅在工作树干净时使用 `git pull --ff-only`，检查差异后刷新客户端。

| 系统 | 入口与说明 | 发现验证 |
|---|---|---|
| Codex | 共享目录 `~/.agents/skills/autoresearch-pro/`；使用 `$autoresearch-pro` | 技能选择器；支持时使用 `codex debug prompt-input` |
| Claude Code | `~/.claude/skills/autoresearch-pro/` 指向共享目录；使用 `/autoresearch-pro` | 新会话的斜杠命令选择器 |
| Hermes | 默认 `~/.hermes/skills/autoresearch-pro/` 指向共享目录；自定义 profile 使用对应的 `$HERMES_HOME` | `hermes skills list --source local --enabled-only`，再通过 `skill_view` 读取 |
| OpenClaw | 当前版本可发现个人共享目录；无法发现时使用原生安装器建立受管副本 | `openclaw skills info autoresearch-pro --agent <id> --json` |

macOS/Linux 为 Claude Code 和 Hermes 建立入口，执行前确认目标不存在：

```sh
mkdir -p ~/.claude/skills "${HERMES_HOME:-$HOME/.hermes}/skills"
ln -s "$HOME/.agents/skills/autoresearch-pro" "$HOME/.claude/skills/autoresearch-pro"
ln -s "$HOME/.agents/skills/autoresearch-pro" "${HERMES_HOME:-$HOME/.hermes}/skills/autoresearch-pro"
```

Claude 的目录访问权限不是技能入口。Hermes 应在目标 profile 中保持技能启用。OpenClaw 先检查原生发现；必要时使用：

```sh
openclaw skills install "$HOME/.agents/skills/autoresearch-pro" --global
openclaw skills info autoresearch-pro --agent <id> --json
```

将 `<id>` 替换为实际 Agent ID，检查 `eligible`、`modelVisible`、`commandVisible`。不要强制覆盖不同内容的既有安装。软链接和安装器行为随客户端版本变化；不能使用链接时，整体复制 `SKILL.md` 和 `references/`，记录来源 commit 并显式同步。

核心规则只有标准 frontmatter 和 Markdown，不依赖特定客户端的插件、Hook 或工具名。实际编辑与测试使用当前客户端已有工具；纯聊天环境只能交付有边界的文本对照，不能声称运行验证通过。

## 旧名迁移

先将旧版备份移出活动技能根目录，再比较本地改动、更新有效目录与显式路径/调用引用。共享链接也应改指新目录，避免旧版同时被发现。历史记录、上游来源和注册表元数据保留原名。ClawHub 保持已有的 `0xcjl/autoresearch-pro` slug，独立更新版本。GitHub 使用 MIT，ClawHub 经维护者授权使用平台 MIT-0。GitHub 更新不自动证明注册表版本已经更新，应核实具体版本。

## 验证边界

2026-10-03 使用五个案例对照：两个正常优化、两个相邻任务、一个未参与改写的离线新案例。三个优化任务中，旧版停在确认/清单选择，新版形成可审阅产物；两个相邻任务均正确路由，因此不声称触发准确率提高。

分类标签是会话内实际输出；流程仅做文本检查；新案例由同一评估者顺序测试，不是独立盲测。没有生产准确率、跨模型效果或统计性提升结论。

本机 Codex、Claude Code、Hermes、OpenClaw 的原生发现已检查，Hermes 还成功读取正文和参考文件；不等于每个客户端都完成真实模型行为测试。静态安全扫描为 SAFE / 8，唯一 EA2 指向“不要求用户选择通用清单”的句子；维护者明确接受，未抑制发现，重要外部操作仍须明确授权。

来源链接和 MIT 许可证见 [英文说明](README.md#sources-and-license)。
