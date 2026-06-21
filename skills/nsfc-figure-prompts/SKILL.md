---
name: nsfc-figure-prompts
description: >
  Generate AI drawing prompts for scientific figures, architecture diagrams, grant charts, and academic illustrations. Use whenever the user asks for 绘图提示词, AI绘图, figure prompts, 画图 prompt, wants to describe scientific figures for AI image generation, or needs prompts for creating academic architecture diagrams. Also trigger on requests like 帮我写个图的提示词, 生成图片描述, 科研绘图.
---

# Scientific Figure Prompt Generator

Generate prompts that produce consistently excellent scientific figures.
**Methodology learned from a proven 8-figure NSFC template that went through 7 rounds of iteration.**

## Core Design Philosophy (learned from user feedback)

### The "共同体" concept — why dashed lines matter

Dashed boxes are NOT decorative. They are a semantic tool: they tell the viewer "these elements belong together as a group." Every dashed box should have a small label in its corner naming the group (e.g., "实验条件(5组)"、"评估指标(三维)"). This creates visual hierarchy without heavy borders. **Always use dashed boxes to group related elements.**

### Three shapes, no more

- 圆角矩形 = modules, channels, boxes
- 圆形 = hub nodes, central engines
- 菱形 = milestones only

Never introduce random shapes (hexagons, triangles, stars). Shape variety should serve meaning, not decoration.

### The bottom bar signature

Every figure ends with a deep blue horizontal bar containing a one-line white-text summary. This is the visual signature that makes all figures feel like one cohesive set. The reviewer can scan bottom bars across all figures and instantly understand the full story.

### Flat academic aesthetic

No gradients. No shadows. No 3D effects. Pure white background. This is an NSFC grant application, not a startup pitch deck. Academic reviewers distrust visual decoration.

### Information density: high but not crowded

Every module must contain at least **title + description** (two layers of text). A module with just a single-word title is undercooked. But don't fill every pixel — leave breathing room. The reference template achieves this by having 3-5 modules per figure, each with 2-4 lines of content.

### Color carries meaning

Colors are not aesthetic choices — they encode the conceptual layer:
- Blue → cognitive/perception layer (RC1, PINN-SA)
- Green → decision layer (RC2, HITL-COLREGS, CASE-SEA, EXPLAIN)
- Coral → execution/safety layer (RC3, GUARDIAN)

When writing prompts, always specify color by FUNCTION: "深蓝边框" not "#1A6B8A", "浅绿底色" not "#E8F6F0". AI generators understand natural color names better than hex codes.

### Arrows tell the story

Every arrow/connector needs a small text annotation explaining WHAT flows in that direction. Not just an arrow — an arrow labeled "态势向量→" or "奖励信号反馈←". This is how the figure becomes self-explanatory to a reviewer who hasn't read the article.

### Merged figures use clear dividers

When two concepts share one figure (e.g., PINN-SA + NSA-SSM in Fig 3), use:
```
===== 上半部分：XXX =====
===== 下半部分：YYY =====
```
With a separator line and transition annotation between them.

## Prompt Structure

For each figure, output ONE code block:

```markdown
## 图N：标题

```
绘制一张学术架构图，纯白背景，中文。

核心信息：[一句话，审稿人看图必须带走的结论]

布局方向：[空间组织 — 纵向堆叠/中心环绕/三列并排/环形/上下组合/二维坐标]

[逐模块：
  - 位置（上方/下方/左右/中央）+ 形状 + 配色
  - 精确文字（用引号包裹）
  - 子元素（标题栏、内部小框、标注）
  - 虚线框分组（含分组标签）]

[箭头和连接线，每条旁注小字说明流向]

底部深蓝总结条白字："[核心信息总结]"。
```
```

## Quality Checklist (verify before output)

- [ ] 核心信息已写在开头（一句话）
- [ ] 布局方向已明确（空间关系词，非坐标）
- [ ] 每个模块的精确中文文字已写入（标题+正文两层）
- [ ] 配色用颜色名指定（蓝/绿/珊瑚/灰），非色号
- [ ] 虚线框用于分组，有分组标签
- [ ] 箭头有文字标注流向
- [ ] 底部有深蓝总结条白字
- [ ] 全局约束：纯白背景、无阴影渐变3D
- [ ] 只用了三种形状（圆角矩形/圆形/菱形）
- [ ] 信息密度：每个模块≥2层文字，整体不拥挤
- [ ] 方法名拼写检查

## Color Convention

For this NSFC project — always use this mapping:

| 概念层 | 边框 | 底色 | 标题栏 | 文字 |
|--------|------|------|--------|------|
| 认知层(RC1) | 深蓝边框 | 浅蓝底色 | 蓝底白字 | 深蓝加粗 |
| 决策层(RC2) | 绿边框 | 浅绿底色 | 绿底白字 | 绿加粗 |
| 执行层(RC3) | 珊瑚边框 | 浅珊瑚底色 | 珊瑚底白字 | 珊瑚加粗 |
| 辅助/分组 | 灰虚线 | 透明 | — | 灰色小字 |

## Layout Archetypes

| Archetype | When | Key phrase |
|-----------|------|-----------|
| 纵向堆叠 | Layered architecture | "纵向排列，自上而下，层间箭头" |
| 中心环绕 | Hub-and-spoke | "中央圆形，外围模块环绕，灰线连接" |
| 左右三列 | Flow/I/O | "左中右三列并排，左→中→右流" |
| 环形 | Cycle/loop | "顺时针排列，弧形箭头连接，闭环" |
| 上下组合 | Two related concepts | "===== 上半部分 ===== / ===== 下半部分 =====" |
| 二维空间 | Zone/matrix | "坐标系，横轴/纵轴，分区，轨迹线" |

## Reference

`Reference.md`

Read it to calibrate tone, density, and detail level before generating new prompts.
