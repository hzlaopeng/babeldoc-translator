# BabelDOC PDF Translator Skill

__用 Claude Code 翻译 PDF 科学论文，像专业人士一样简单__

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-purple.svg)](https://claude.ai/claude-code)

##  这是什么？

__BabelDOC Translator Skill__ 是一个让你的 Claude Code 具备专业 PDF 翻译能力的技能插件。

>  一句话理解：在 Claude Code 中直接翻译 PDF，保持格式、术语表管理、双语对照

##  快速开始

### 安装 BabelDOC

```bash
# 使用 uv 安装（推荐）
pip install uv
uv tool install --python 3.12 BabelDOC
```

### 配置 API

编辑 `~/.claude/babeldoc.toml`：

```toml
[babeldoc]
lang-in = "en-US"
lang-out = "zh-CN"
openai = true
openai-model = "glm-4-flash"
openai-base-url = "https://open.bigmodel.cn/api/paas/v4"
openai-api-key = "your-api-key-here"
```

### 在 Claude Code 中使用

```
翻译 input/paper.pdf
```

##  核心功能

-  PDF 科学论文翻译，保持公式和格式
-  双语对照和纯译文输出
-  术语表管理，确保专业术语一致
-  批量处理多个文档

##  项目结构

```
babeldoc-translator/
├── SKILL.md              # 技能定义
├── README.md             # 项目说明
├── templates/            # 配置模板
│   ├── basic.toml
│   ├── full.toml
│   └── glossary_template.csv
└── docs/                 # 详细文档（如果需要）
```

##  使用方法

```
# 翻译单个 PDF
翻译 input/paper.pdf

# 批量翻译
翻译 input/*.pdf

# 使用术语表
翻译 paper.pdf --glossary-files terms.csv
```

##  配置选项

| 模型 | 特点 |
| --- | --- |
| `glm-4-flash` | 速度快 |
| `deepseek-chat` | 性价比高 |
| `gpt-4o-mini` | 质量稳定 |

##  常见问题

**Q: 支持哪些语言？**

A: 支持所有主流语言对，如英→中、中→英、英→日等。

**Q: 翻译质量如何？**

A: 使用专业 LLM 模型，成功翻译率 94%+。

##  更多资源

- [BabelDOC GitHub](https://github.com/funstory-ai/BabelDOC)

##  许可证

MIT License
