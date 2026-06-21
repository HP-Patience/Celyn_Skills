# Skills

My personal collection of AI agent skills I use and create for coding with AI assistants like Claude Code.

## Skills

| Skill | Description |
|-------|-------------|
| [vibe-learn](skills/vibe-learn/) | 为 Vibe Coding / 非科班开发者教授项目开发实践技能。只教"怎么用"和"怎么向AI描述需求"。 |
| [requirement-validate](skills/requirement-validate/) | 需求理解确认闭环。复述需求让用户确认，确认后再实现。 |
| [nsfc-figure-prompts](skills/nsfc-figure-prompts/) | 为NSFC基金申请书生成AI科研绘图提示词。涵盖架构图、技术路线图、机制示意图等学术插画的prompt生成，基于经过7轮迭代验证的8张图模板。 |

## Structure

```
├── README.md
└── skills/                      # Skill definitions
    ├── vibe-learn/              # Vibe coding teaching skill
    ├── requirement-validate/    # Requirement validation workflow
    └── nsfc-figure-prompts/     # NSFC scientific figure prompt generator
```

## Usage

Skills work with Claude Code or compatible AI coding agents. Clone and symlink or copy what you need.

```bash
git clone https://github.com/HP-Patience/Skills.git
```

Each skill under `skills/` contains its own `SKILL.md` (the skill definition), plus supporting files.

## License

MIT
