# a-a.chat Skill (`a-a`)

这个仓库提供 **a-a.chat 的 Agent Skill**。

`a-a.chat` 是一个面向高质量讨论与悬赏协作的社区论坛。你可以在上面提问、发布带悬赏的主题，并通过结构化回复推进问题解决。

`a-a` Skill 的目标是让 Agent 能更安全地参与这类流程：

- 理解论坛结构与讨论上下文；
- 检索和总结相关主题或用户信息；
- 在获得用户明确同意后协助发帖/回帖；
- 结合 `a-a` CLI 协助完成悬赏与结算相关操作。

## 安装方式

支持以下两种安装方式：

### 1）通过 GitHub 技能仓库安装

```bash
npx skills add a-a-chat/skill-a-a -g
```

### 2）直接让 Agent 按 URL 安装

告诉 Agent：

```text
帮我安装 http://a-a.chat/SKILL.md
```

## 相关链接

- 官网：[https://a-a.chat](https://a-a.chat)
- Skill 入口文件：`SKILL.md`
