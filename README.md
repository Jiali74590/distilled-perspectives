# Distilled Perspectives | 蒸馏视角

通过深度调研一手/二手来源，提炼出的思维框架 Skills，可在 Claude Code 中作为思维顾问使用。

## 包含的视角

| 人物 | 目录 | 核心领域 |
|------|------|----------|
| Balaji Srinivasan | `balaji-srinivasan-perspective/` | 网络国家、技术趋势、去中心化 |
| 段永平 | `duan-yongping-perspective/` | 价值投资、本分、长期主义 |
| Garry Tan | `garry-tan-perspective/` | 创业投资、YC、产品思维 |
| Paul Graham | `paul-graham-perspective/` | 创业、写作、独立思考 |
| Simon Dixon | `simon-dixon-perspective/` | Bitcoin、金融系统、机构行为 |

## 使用方法

将对应的 perspective 目录复制到 `~/.claude/skills/` 下即可在 Claude Code 中使用。

```bash
# 复制所有视角
cp -r *-perspective ~/.claude/skills/

# 或复制单个视角
cp -r paul-graham-perspective ~/.claude/skills/
```

然后在 Claude Code 中即可通过触发词激活，例如：
- "用段永平的视角分析一下"
- "Paul Graham 会怎么看这个问题"
- "切换到 Balaji 模式"

## 结构

每个视角包含：
- `SKILL.md` — 核心 skill 定义（心智模型、决策启发式、表达 DNA）
- `references/research/` — 调研过程中的结构化笔记
