# Skills

My personal collection of AI agent skills I use and create for coding with AI assistants like Claude Code.

## Skills

| Skill | Description |
|-------|-------------|
| [vibe-learn](skills/vibe-learn/) | 为 Vibe Coding / 非科班开发者教授项目开发实践技能。只教"怎么用"和"怎么向AI描述需求"。 |
| [requirement-validate](skills/requirement-validate/) | 需求理解确认闭环。复述需求让用户确认，确认后再实现。 |

## Structure

```
├── README.md
└── skills/             # Skill definitions
    ├── vibe-learn/     # Vibe coding teaching skill
    └── requirement-validate/  # Requirement validation workflow
```

## Usage

Skills work with Claude Code or compatible AI coding agents. Clone and symlink or copy what you need.

```bash
git clone https://github.com/HP-Patience/Skills.git
```

Each skill under `skills/` contains its own `SKILL.md` (the skill definition), plus supporting files.

## License

MIT
