# Creart Skill

图像相关 Claude Code 技能集合。当前包含两个技能：

| 技能 | 路径 | 定位 |
|------|------|------|
| **creart-prompt** | `skills/creart-prompt/` | 高质量图像提示词顾问（Advisor Only），只写 prompt 不出图。复刻自 `@conardli/garden-skills` → `skills/gpt-image-2`，复刻规则见 `garden-replicate.md` |
| **creart-image** | `skills/creart-image/` | 调用 Creart AI API 生成图像。支持多种模型和分辨率，直接出图 |

## 目录结构

```
creart-skill/
├── skills/
│   ├── creart-prompt/
│   │   ├── SKILL.md              # Advisor-only 技能定义
│   │   └── references/           # 结构化提示词模板（16 类 + 89 个文件）
│   └── creart-image/
│       ├── SKILL.md              # 图像生成技能定义
│       ├── references/           # prompt 工程参考、风格预设、模型指南
│       └── scripts/generate.py   # 图像生成脚本
├── scripts/
│   ├── diff-references.sh        # upstream references 同步 & 安全扫描
│   └── sync-filter.txt           # 同步过滤规则
├── garden-replicate.md           # 从 upstream 复刻/增量更新的标准流程
└── .garden-sync.json             # upstream 同步状态跟踪
```

## 复刻流程

`garden-replicate.md` 是从 upstream 复刻 `creart-prompt` 的标准操作手册，由「复刻提示词」流程驱动。它定义了三阶段流程：

1. **验证** — 确认 upstream 身份（`@conardli/garden-skills`）、HEAD SHA、references 安全扫描（dry-run）、SKILL.md 核心内容完整
2. **执行** — 用 `scripts/diff-references.sh` 同步 references（按 `scripts/sync-filter.txt` 过滤不适用分类）、从 upstream SKILL.md 提取生成 advisor-only 版本、更新 `.garden-sync.json`
3. **审查** — references 完整性、不适用分类检查、SKILL.md 结构与残留检查、路径替换验证、跟踪文件有效性

审查结果按分级报告：`[MUST]` 阻塞交付、`[WARN]` 建议调整、`[INFO]` 仅供参考。
