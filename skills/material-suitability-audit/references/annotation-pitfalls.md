# 教学重构版生成 · 实现陷阱速查

生成教学重构版时，通过 Python 脚本向原文插入标注块（🔑💭💡✏️🔗📦）。以下是在第2章实践中踩过的坑和修复方法。

## 1. LaTeX 转义字符破损

### `\varepsilon` → VT 字符（0x0B）

**现象**：文件中的 `\varepsilon` 显示为 `<VT>arepsilon` 或 `\u000barepsilon`，Obsidian 无法渲染。

**原因**：Python 字符串中 `\v` 是垂直制表符（VT, 0x0B）的转义序列。

**错误写法**：
```python
text = "\\varepsilon"  # \\v 被解释为 \ + v，但写入文件时可能出问题
raw = b'\\varepsilon'   # 在某些上下文中 \v 仍是 VT
```

**正确写法**——用十六进制字节显式指定：
```python
correct = b'\x5c\x76arepsilon'  # 0x5C = \, 0x76 = v → \varepsilon
broken  = b'\x0barepsilon'      # 0x0B = VT
raw = raw.replace(broken, correct)
```

### `\frac` → FF 字符（0x0C）

**现象**：`\frac{1}{2}` 变成 `rac{1}{2}`。

**原因**：`\f` 是换页符（FF, 0x0C）。

**修复**：
```python
correct = b'\x5c\x66rac'  # 0x5C = \, 0x66 = f → \frac
broken  = b'\x0crac'
raw = raw.replace(broken, correct)
```

### 通用检测

生成教学版后，扫描所有控制字符：
```python
for byte_val in [0x0b, 0x0c, 0x00]:
    count = raw.count(bytes([byte_val]))
    if count:
        print(f"WARNING: 0x{byte_val:02x} found {count} times")
```

## 2. 标注块放置位置

### 🔑💭：标题内 vs 标题外

❌ 放在节标题**之前**：
```
🔑 核心问题...
💭 引导思考...
## 2.3 平稳性        ← 标题在标注后面
```

✅ 放在节标题**之下、正文第一行之前**：
```
## 2.3 平稳性

🔑 核心问题...
💭 引导思考...
[正文开始]
```

**修复脚本**（移动所有 🔑💭 到对应标题内）：
```python
for heading, core_start in sections:
    idx_core = c.find(core_start)
    idx_heading = c.find(heading, idx_core)
    block = c[idx_core:idx_heading].rstrip()
    c = c[:idx_core].rstrip() + "\n\n" + heading + "\n\n" + block + c[idx_heading + len(heading):]
```

### 💡✏️：段落之间 vs 句子中间

❌ 插在句子中间：
```
...相关关系的度量
> 💡 因果链...
> ✏️ 请动笔...
，而某种程度上...    ← 半句话被截断
```

✅ 插在段落之间：
```
[完整段落结束]

> 💡 因果链...
> ✏️ 请动笔...

[下一段开始]
```

**排查方法**：检查每个 💡✏️ 块的上一行和下一行是否都是完整段落，而不是残缺句子。

## 3. 抽象→具体 翻转判断

不是所有节都需要翻转。判断标准：

| 翻转 | 不翻转 |
|---|---|
| 概念本身是"抽象定义→子结论"（严平稳→弱平稳）| 具体模型"公式→推导性质"（随机游动、滑动平均）|
| 正文从抽象定义出发 | 正文从具体公式出发 |

**第2章实践**：只有 2.3（平稳性）和 2.3.1（白噪声）需要翻转。其他 5 节不动。

## 4. R 代码块格式

原文中 R 代码可能以 `>` 引用块形式出现：
```
> plot(rwalk, type='o', ylab='Random Walk')
```

教学版必须转为 ```` ```r ```` 代码块并加说明行：
````markdown
> 📎 **配套 R 代码**（考试不要求写代码，仅作参考）

```r
plot(rwalk, type='o', ylab='Random Walk')
```
````
