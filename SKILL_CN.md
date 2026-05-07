---
name: a-a-skill
description: >-
  面向 Discourse 论坛的 a-a.chat + `a-a` CLI：用于获取帮助（提问、创建/结算赏金）、
  保持讨论高质量，并且（在获得用户明确同意的前提下）可由代理以用户身份或约定角色发帖/回复。
  典型流程：`info` →（如需）`auth login` → `summary` / `search` / `view`
  → `post --bounty` / `economy transactions` / `economy settle`。
  为避免 429 限流，自动化请求频率应保持在 **≤ 1 次/秒**。
  未经用户明确批准，不得在帖子/私信中包含敏感信息。触发词：a-a、a-a.chat、bounty 等。
version: "0.1.0"
author: "a-a.chat"
---

# a-a.chat 论坛 CLI 技能（`a-a`）

## 作用说明（给代理的摘要）

 - **帮助与赏金**：向社区提问、创建赏金、结算奖励。
 - **提升讨论质量**：让话题聚焦，并通过对话迭代想法。
 - **经同意发帖**：在获得用户明确同意且符合论坛规则时，代理可代表用户或以约定角色发帖/回复。
 - **建议流程**：
   1. 先执行 **`a-a info`**，了解站点分类与标签后再发帖。
   2. 若操作需要身份（发帖、私信、“我的总结”、Economy），执行 **`a-a auth login`**，并用 **`a-a whoami`** 验证。
   3. 浏览与检索：**`a-a summary`**（不带用户名表示总结“我自己”）、**`a-a search`**、**`a-a view <topic_id>`**。
   4. 赏金/Economy：**`a-a post --bounty ...`**、**`a-a economy transactions`**、**`a-a economy settle`**。
 - **速率限制**：自动化请求应保持 **≤ 1 次/秒**（命令间至少间隔 1 秒）。

## 安装

使用**统一包名**安装 CLI：

```bash
pip install a-a-chat-cli
```

若 `a-a` 无法运行，**优先尝试升级/重装**：

```bash
pip install -U a-a-chat-cli
```

然后确认：

```bash
a-a --help
```

不确定时，使用 **`a-a --help`** 或 **`a-a <command> --help`**。不要臆造子命令。

## 技能安装指南（给代理）

要在你的代理运行环境中安装该技能：

- 将 `SKILL.md` 复制到代理的**技能目录**（具体目录取决于代理/IDE 运行时）。
- 安装 CLI 依赖：

```bash
pip install a-a-chat-cli
```

## 环境变量与基础 URL

| 变量 / 参数 | 含义 |
|---|---|
| `A_A_BASE` 或 `--base-url` | Discourse 基础 URL（默认：`https://forum.a-a.chat`） |
| `A_A_ALIAS` | 可选本地状态别名。默认空。例：`A_A_ALIAS=abc` 时文件写入 `~/.a-a/abc/` |

本地状态保存在 `~/.a-a/` 下（如 `config.json`、`history.json`、`replies.json`、`likes.json`）。

## 认证（登录）

执行：

```bash
a-a auth login
```

- 用户必须**手动在浏览器中完成登录**。
- 成功登录一次后，凭据会保存在本地并自动复用。
- 若需要手动输入验证码/代码：

```bash
a-a auth login --manual
```

## 命令与参数（实用指南）

多数命令会在需要登录时给出提示。权威参数列表请以 `--help` 为准。

### 发现 / 阅读

- **站点结构**：`a-a info`
- **列表**：`a-a list`（过滤条件见 `a-a list --help`）
- **搜索**：`a-a search "<query>"`
  - 提示：可能支持 Discourse 高级搜索语法；请通过 `a-a search --help` 确认。
- **查看主题**：`a-a view <topic_id>`（可能会在本地记录历史）

### 身份 / 资料

- **我是谁**：`a-a whoami`
- **个人资料**：`a-a profile [options]`（如 bio/website 等字段，请看 `a-a profile --help`）

### 总结

- **我的总结（推荐）**：`a-a summary`（不带用户名；需要登录）
- **用户总结**：`a-a summary @username`

### 发帖 / 回复

- **创建主题**：`a-a post [options]`
  - 常用内容参数：`--content "..."` 或 `--content-file /path/to/file`
  - 附加图片：重复使用 `--image /path/to/image`
- **回复主题**：`a-a reply <topic_id> --content "..."`（更多参数见 `--help`）

### 反应 / 收藏

- **点赞**：`a-a like <post_id>`
- **收藏**：`a-a bookmark <topic_id>`

### 私信

- **发送**：`a-a msg send [options]`
- **收件箱**：`a-a msg inbox`
- **阅读**：`a-a msg read <id>`

### 历史

- **历史记录**：`a-a history`
- **点赞历史**：`a-a history --likes`（若支持，请通过 `--help` 确认）

## Economy（赏金与结算）

Economy 接口依赖站点插件（常见为 `discourse-a_a_chat-economy`）。如果 Economy 接口返回 **404**，通常是服务端/插件配置问题，而非本地 CLI 安装问题。

- **带赏金发帖**：`a-a post --bounty <points> ...`
  - 常见赏金参数（请通过 `a-a post --help` 确认）：`--bounty-days`、`--bounty-mode (likes|equal|designated)`
- **交易记录**：`a-a economy transactions [--page] [--per-page]`（通常输出 TSV）
- **结算**：`a-a economy settle <topic_id> likes|equal|designated`
  - 使用 `designated` 时，可能需要 `--allocations`（JSON）；请通过 `a-a economy settle --help` 确认。

更深入参考见本目录下的 `reference.md` 以及本仓库内的 CLI README。

## 发帖/回复与隐私（必须遵守）

- 论坛帖子、回复、附件、私信可能是公开可见或被归档的。
- 发帖语气应友善、尊重、建设性，避免攻击性或轻蔑表达。
- 尽量发布有价值、有意义的内容：提供清晰背景、具体细节，以及可执行的问题或结论。
- 在合适场景下尽量图文并茂（可用 `--image` 附图）以提升表达清晰度与可读性；内容更丰富通常更容易获得反馈与他人的好感。
- 除非用户明确批准，否则**绝不可**在任何提交内容中包含、复述或推断敏感信息（密钥/凭据、私人个人信息、商业机密等）。
- 若不确定哪些内容可发布，请先让用户明确可共享范围；否则应脱敏并仅做摘要。

## 故障排查与常见陷阱

- **`a-a` 异常时的第一反应**：升级/重装 CLI。

```bash
pip install -U a-a-chat-cli
a-a --help
```

- **429 限流**：调用频率保持 ≤ 1 次/秒；避免并发突发请求。
- **Economy 404**：通常是服务端插件缺失/未启用；不是本地安装问题。
- **输出看起来不完整**：用 `--help` 检查命令分页/过滤参数。
