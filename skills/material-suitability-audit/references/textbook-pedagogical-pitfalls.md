# Textbook Teaching Edition Implementation Pitfalls

> Condensed from multi-session experience generating pedagogical restructured versions (教学重构版) from OCR-repaired textbook originals.

## Pitfall 1: 💡/✏️ Block Placement — Between Paragraphs, Never Mid-Sentence

**Symptom**: Annotations like `> 💡 **因果链**...` or `> ✏️ **请动笔**...` appear embedded in the middle of a sentence, splitting the original text's grammar.

**Cause**: The insertion anchor (a keyword match) landed inside a paragraph rather than at a paragraph boundary.

**Correct placement**:
- `💡 因果链` goes AFTER the derivation/formula block it annotates, BEFORE the next topic begins — between paragraphs.
- `✏️ 自查` goes AFTER the entire concept's body (📖 正文), BEFORE the next concept's 🔑 section.

**Wrong** (mid-sentence):
```
...相关关系的度量
> 💡 因果链...
> ✏️ 请动笔...
，而某种程度上...          ← 半句话被拦腰截断
```

**Right** (between paragraphs):
```
...相关关系的度量，而某种程度上...

> 💡 因果链...
> ✏️ 请动笔...

下一个公式...
```

**Verification**: After insertion, read the 3 lines before and after each annotation. If the original text flows continuously through the annotation, the placement is wrong.

## Pitfall 2: Python String Escaping — LaTeX Backslash-Letter Combos

**Symptom**: LaTeX commands like `\varepsilon`, `\frac`, `\varphi` appear broken in generated files — showing as `arepsilon`, `rac`, or with invisible control characters like `^K` (VT, 0x0B) or `^L` (FF, 0x0C).

**Cause**: Python interprets certain `\letter` sequences as escape characters:
| LaTeX | Python escape | Byte | Symptom |
|---|---|---|---|
| `\v` (in `\varepsilon`) | Vertical Tab | 0x0B | `arepsilon` or `^Karepsilon` |
| `\f` (in `\frac`) | Form Feed | 0x0C | `rac` or `^Lrac` |

Even `r"..."` raw strings don't fully protect against this when concatenating or reading/writing across encodings.

**Fix**: When doing binary replacements, use **hex bytes** for the backslash-letter sequence:

```python
# DO NOT USE — \v becomes VT
correct = b'\\varepsilon'  # ❌ \v → 0x0B

# USE HEX BYTES INSTEAD
broken = b'\x0barepsilon'            # VT + 'arepsilon'  
correct = b'\x5c\x76arepsilon'       # \ (0x5C) + v (0x76) + arepsilon

raw = raw.replace(broken, correct)
```

Common hex mappings for LaTeX fixes:
| LaTeX | Hex |
|---|---|
| `\v` | `\x5c\x76` |
| `\f` | `\x5c\x66` |
| `\t` | `\x5c\x74` |
| `\n` | `\x5c\x6e` |
| `\r` | `\x5c\x72` |

**Post-fix verification**:
```bash
# Check for remaining control characters
python3 -c "
with open('file.md', 'rb') as f:
    raw = f.read()
for b in [0x0b, 0x0c, 0x00]:
    print(f'0x{b:02x}: {raw.count(bytes([b]))}')
"
```

## Pitfall 3: R Code in Blockquotes → Code Blocks

**Symptom**: Original textbook files have R code wrapped in `> ` blockquote markers rather than ```` ```r ```` code blocks. Obsidian won't syntax-highlight these.

**Context**: Exam requires reading software output, not writing code. Keep code but add a note.

**Fix**:
```markdown
> 📎 **配套 R 代码**（考试不要求写代码，仅作参考）

```r
win.graph(width=4.875, height=2.5, pointsize=8)
data(rwalk)
plot(rwalk, type='o', ylab='Random Walk')
```
```

## Pitfall 4: insert_before → Contents Must Be Inside Section Headers

**Symptom**: `🔑` and `💭` blocks appear BEFORE `##` or `###` headings rather than inside the section.

**Correct**: Every concept's `🔑💭` pair belongs INSIDE the section — after the heading, before the body text.

```
### 2.2.1 随机游动

🔑 **核心问题**：...
💭 **引导思考**...
[正文开始]
```
