# BabelDOC PDF Translator Skill

__用 Claude Code 翻译 PDF 科学论文，保持公式和格式__

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-purple.svg)](https://claude.ai/claude-code)

##  这是什么？

**BabelDOC Translator Skill** 是一个 Claude Code 技能，让你在 Claude Code 中直接翻译 PDF 科学论文。

> 一句话理解：用自然语言告诉 Claude 翻译 PDF，自动保持公式、图表和格式

---

##  核心功能

-  PDF 科学论文翻译，保持公式和格式
-  双语对照和纯译文输出
-  术语表管理，确保专业术语一致
-  批量处理多个文档

---

##  快速开始

### 方式一：直接克隆（最简单）

```bash
# 克隆到技能目录
git clone https://github.com/pengdewen/babeldoc-translator.git ~/.claude/skills/babeldoc-translator
```

然后在 Claude Code 中直接使用即可。

### 方式二：手动复制

```bash
# 先克隆到任意位置
git clone https://github.com/pengdewen/babeldoc-translator.git

# 复制到技能目录
cp -r babeldoc-translator ~/.claude/skills/
```

### 方式三：符号链接（便于开发）

```bash
# 克隆到你的工作目录
git clone https://github.com/pengdewen/babeldoc-translator.git

# 创建符号链接
ln -s $(pwd)/babeldoc-translator ~/.claude/skills/babeldoc-translator
```

---

##  安装 BabelDOC

在使用技能之前，需要先安装 [BabelDOC](https://github.com/funstory-ai/BabelDOC)：

### Windows

```bash
# 安装 uv（如果还没有）
pip install uv

# 安装 BabelDOC
uv tool install --python 3.12 BabelDOC

# 验证安装
babeldoc --help
```

### Mac/Linux

```bash
# 安装 uv
pip install uv

# 安装 BabelDOC
uv tool install --python 3.12 BabelDOC

# 添加到 PATH（如果还没添加）
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc  # 或 ~/.zshrc
source ~/.bashrc

# 验证安装
babeldoc --help
```

---

##  配置 API

创建配置文件 `~/.claude/babeldoc.toml`：

```toml
[babeldoc]
lang-in = "en-US"
lang-out = "zh-CN"
openai = true
openai-model = "glm-4-flash"
openai-base-url = "https://open.bigmodel.cn/api/paas/v4"
openai-api-key = "your-api-key-here"
```

> ⚠️ **安全提醒**: 不要将包含真实 API Key 的配置文件提交到 Git 或公开仓库。

### 使用配置模板

项目提供了配置模板，可直接复制使用：

```bash
# 基础配置模板
cp templates/basic.toml ~/.claude/babeldoc.toml

# 完整配置模板（包含更多选项）
cp templates/full.toml ~/.claude/babeldoc.toml
```

复制后，编辑 `~/.claude/babeldoc.toml`，填入你的 API Key。

### 推荐模型

| 模型 | 特点 | 适用场景 |
| --- | --- | --- |
| `glm-4-flash` | 速度快 | 日常翻译 |
| `deepseek-chat` | 性价比高 | 大批量翻译 |
| `gpt-4o-mini` | 质量稳定 | 专业论文 |

---

##  使用方法

安装技能后，在 Claude Code 中直接对话：

```
翻译 input/paper.pdf

批量翻译 ./input/*.pdf

使用术语表 terms.csv 翻译 paper.pdf
```

### 使用术语表

术语表可以确保专业术语翻译的一致性：

```bash
# 1. 复制术语表模板
cp templates/glossary_template.csv my_terms.csv

# 2. 编辑术语表，添加专业术语
# source,target,tgt_lng
# Transformer,Transformer,ja
# Attention,注意力机制,zh

# 3. 在 Claude Code 中使用
使用 my_terms.csv 术语表翻译 paper.pdf
```

### 项目结构建议

```
my-translation-project/
├── babeldoc.toml          # 配置文件
├── input/                 # 输入 PDF 目录
│   └── paper.pdf
├── output/                # 输出翻译结果目录
└── glossary/              # 术语表目录
    └── terms.csv
```

---

##  项目结构

```
babeldoc-translator/
├── SKILL.md              # 技能定义文件
├── README.md             # 项目说明
├── LICENSE               # MIT 许可证
└── templates/            # 配置模板
    ├── basic.toml        # 基础配置模板
    ├── full.toml         # 完整配置模板（含所有选项）
    └── glossary_template.csv  # 术语表模板
```

### 模板文件说明

| 文件 | 说明 |
|------|------|
| `basic.toml` | 基础配置，包含常用选项 |
| `full.toml` | 完整配置，包含所有 BabelDOC 选项 |
| `glossary_template.csv` | 术语表模板，用于专业术语翻译 |

---

##  常见问题

**Q: 支持哪些语言？**

A: 支持所有主流语言对，如英→中、中→英、英→日等。

**Q: 翻译质量如何？**

A: 使用专业 LLM 模型，成功翻译率 94%+。

**Q: 如何处理扫描版 PDF？**

A: BabelDOC 会自动检测，必要时启用 OCR 模式。

---

##  安全提醒

⚠️ **重要**：请勿将包含真实 API Key 的配置文件提交到 Git 或公开仓库。

本项目已包含 `.gitignore` 配置，会自动忽略所有 `.toml` 配置文件（除 `templates/` 目录下的模板外）。

如果你的配置文件已经被追踪，请先清除：
```bash
git rm --cached ~/.claude/babeldoc.toml 2>/dev/null || true
```

---

##  更多资源

- [BabelDOC GitHub](https://github.com/funstory-ai/BabelDOC)
- [BabelDOC 文档](https://github.com/funstory-ai/BabelDOC/blob/main/README.md)

---

##  许可证

MIT License - 详见 [LICENSE](LICENSE) 文件
