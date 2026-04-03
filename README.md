# minimal-prompt-tuning-skill

一个用于低 token 成本调优的 Codex skill，适合根据测试报告和 badcase 优化 `system_prompt.txt` 与 `tools_call.json`。

它的目标不是“不断加 prompt”，而是：
- 先归因，再定点修
- 能改 tool 就不改全局 prompt
- 先删重复，再补缺失
- 优先解决高频、稳定、可泛化的问题

## 适用场景

适合处理这几类问题：
- 意图判断不稳
- tool 误路由
- 参数漏填、错填、误保留
- 建议和执行边界混淆
- 退出误判
- 回复风格或播报稳定性问题

## 仓库结构

```text
minimal-prompt-tuning/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── playbook.md
```

## 安装

推荐直接复制到本地 skills 目录：

```bash
git clone git@github.com:bestjane/minimal-prompt-tuning-skill.git
mkdir -p ~/.codex/skills
cp -R minimal-prompt-tuning-skill ~/.codex/skills/
mv ~/.codex/skills/minimal-prompt-tuning-skill ~/.codex/skills/minimal-prompt-tuning
```

最终目录应为：

```text
~/.codex/skills/minimal-prompt-tuning/
```

## 使用方式

在 Codex 中显式提到：

```text
$minimal-prompt-tuning
```

例如：

```text
用 $minimal-prompt-tuning 分析这轮 badcase，判断该改 system_prompt.txt 还是 tools_call.json
```

## 这个 skill 会做什么

- 先按错误类型给 badcase 归因
- 判断问题应落在 `system_prompt.txt` 还是 `tools_call.json`
- 优先建议最小改动，而不是堆叠新规则
- 强调改后复测，并记录收益与副作用

## 设计原则

- 一条规则只表达一个核心判断
- tool 顶层描述只写用途和边界
- 参数描述只写取值条件、保留条件、必填条件
- 跨工具共享规则才进入 `system_prompt.txt`
- 个别样本不值得用大量 token 去修

## 参考说明

- `SKILL.md`：精简入口，减少触发后上下文占用
- `references/playbook.md`：完整方法论与执行手册

## 发布记录

- `v1`：完成 skill 初版、GitHub 分发和中文化元数据

## License

MIT
