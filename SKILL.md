---
name: babeldoc-translator
description: BabelDOC PDF 文档翻译助手 - 提供智能 PDF 科学论文翻译、双语对照、术语管理和批量处理能力
author: Claude Code Assistant
version: 1.0.0
---

# BabelDOC PDF 文档翻译助手

将 [BabelDOC](https://github.com/funstory-ai/BabelDOC) 的强大 PDF 翻译能力集成到 Claude Code 中，专为学术文档、技术论文和科研资料的翻译而设计。

## 核心功能

- 📄 **智能翻译**: 保持格式的 PDF 科学论文翻译
- 🌐 **双语对照**: 原文与译文对照显示
- 📚 **术语管理**: 自动提取术语并应用术语表
- 🔄 **批量处理**: 一次性翻译多个文档
- 💾 **离线支持**: 生成离线资源包用于无网络环境
- ⚙️ **灵活配置**: 支持 TOML 配置文件管理

## 快速开始

### 安装依赖

首先确保安装了 BabelDOC:

```bash
# 使用 uv 安装 (推荐)
uv tool install --python 3.12 BabelDOC

# 验证安装
babeldoc --help
```

### 文件路径说明

**重要**: BabelDOC 使用相对路径或绝对路径来处理文件：

- **输入文件**: 支持相对路径 (`./input/paper.pdf`) 或绝对路径 (`C:\docs\paper.pdf`)
- **输出目录**: 默认为当前目录，使用 `--output` 或配置文件指定
- **配置文件**: 默认查找当前目录的 `babeldoc.toml`

**推荐的项目结构**:
```
my-translation-project/
├── babeldoc.toml          # 配置文件
├── input/                 # 输入 PDF 目录
│   ├── paper1.pdf
│   └── paper2.pdf
├── output/                # 输出翻译结果目录
│   ├── paper1.zh-CN.dual.pdf
│   └── paper2.zh-CN.mono.pdf
├── glossary/              # 术语表目录
│   └── terms.csv
└── logs/                  # 日志目录
```

### 基本用法

```
翻译单个 PDF 文件:
/babeldoc translate input/paper.pdf

指定语言和输出目录:
/babeldoc translate input/paper.pdf --lang-in en --lang-out zh --output ./output

批量翻译多个文件:
/babeldoc batch ./input/*.pdf

使用自定义配置:
/babeldoc translate input/paper.pdf --config ./babeldoc.toml
```

## 命令参考

### /babeldoc translate

翻译单个 PDF 文件。

**用法:**
```
/babeldoc translate <file.pdf> [options]
```

**文件路径规则:**
- 输入文件可以是相对路径 (`./input/paper.pdf`) 或绝对路径
- 输出目录默认为当前目录，建议明确指定 (`--output ./output`)
- 输出文件命名格式: `<原文件名>.<目标语言>.dual.pdf` (双语) / `.mono.pdf` (单语)

**选项:**

| 选项 | 说明 | 默认值 |
|------|------|--------|
| `--lang-in`, `-li` | 源语言代码 | `en` |
| `--lang-out`, `-lo` | 目标语言代码 | `zh` |
| `--output`, `-o` | 输出目录 | 当前目录 |
| `--pages`, `-p` | 翻译指定页码 (如 "1,2,3-5") | 全部 |
| `--no-dual` | 不输出双语 PDF | - |
| `--no-mono` | 不输出单语 PDF | - |
| `--openai-model` | OpenAI 模型 | `gpt-4o-mini` |
| `--qps` | 每秒请求数限制 | `4` |
| `--glossary-files` | 术语表 CSV 文件 (逗号分隔) | - |

**示例:**
```
/babeldoc translate input/research.pdf
/babeldoc translate input/paper.pdf --lang-in en --lang-out zh-CN --output ./output
/babeldoc translate docs/thesis.pdf --pages "1-10,15,20-25" --no-dual
```

### /babeldoc batch

批量翻译多个 PDF 文件。

**用法:**
```
/babeldoc batch <pattern> [options]
```

**文件路径规则:**
- 支持通配符模式 (`./input/*.pdf`, `./papers/**/*.pdf`)
- 每个输入文件保持相对目录结构
- 输出目录统一由 `--output` 指定

**选项:** 与 `translate` 相同

**示例:**
```
/babeldoc batch "./input/*.pdf" --output ./output
/babeldoc batch "./docs/**/*.pdf" --lang-in en --lang-out ja --output ./ja_translated
```

### /babeldoc glossary extract

从 PDF 文件中自动提取术语，生成术语表。

**用法:**
```
/babeldoc glossary extract <file.pdf> [options]
```

**文件路径规则:**
- 输入为单个 PDF 文件路径
- 输出为 CSV 文件路径，默认保存到当前目录
- 建议将术语表保存在 `glossary/` 目录中便于管理

**选项:**

| 选项 | 说明 |
|------|------|
| `--output`, `-o` | 输出 CSV 文件路径 |
| `--min-count` | 最小出现次数阈值 |

**示例:**
```
/babeldoc glossary extract input/paper.pdf --output glossary/terms.csv
```

### /babeldoc glossary create

创建一个新的空白术语表模板。

**用法:**
```
/babeldoc glossary create <filename.csv>
```

生成的 CSV 包含以下列:
- `source`: 原语言术语
- `target`: 目标语言术语
- `tgt_lng`: 目标语言代码 (可选)

**示例:**
```
/babeldoc glossary create custom_terms.csv
```

### /babeldoc config init

在当前项目初始化 BabelDOC 配置文件。

**用法:**
```
/babeldoc config init [options]
```

**选项:**

| 选项 | 说明 |
|------|------|
| `--output`, `-o` | 配置文件输出路径 | `babeldoc.toml` |
| `--template` | 配置模板类型 | `basic` \| `full` |

**示例:**
```
/babeldoc config init
/babeldoc config init --template full --output ./configs/translation.toml
```

### /babeldoc offline package

生成离线资源包，用于无网络环境。

**用法:**
```
/babeldoc offline package <output_directory>
```

**示例:**
```
/babeldoc offline package ./offline_assets
```

### /babeldoc offline restore

从离线资源包恢复。

**用法:**
```
/babeldoc offline restore <package.zip>
```

**示例:**
```
/babeldoc offline restore ./offline_assets/offline_assets_*.zip
```

## 配置文件格式

生成的 `babeldoc.toml` 配置文件示例:

```toml
[babeldoc]
# 基础设置
debug = false
lang-in = "en-US"
lang-out = "zh-CN"
qps = 4
output = "./translated"

# PDF 处理选项
pages = ""
max-pages-per-part = 50
skip-scanned-detection = false
ocr-workaround = false
watermark-output-mode = "watermarked"  # watermarked | no_watermark | both

# 翻译服务
openai = true
openai-model = "gpt-4o-mini"
openai-base-url = "https://api.openai.com/v1"
openai-api-key = "your-api-key-here"

# 术语表 (可选)
# glossary-files = "./terms.csv"

# 输出控制
no-dual = false
no-mono = false
min-text-length = 5

# 性能
pool-max-workers = 8
```

## 工作流程示例

### 学术论文翻译流程

```
1. 初始化项目配置
   /babeldoc config init

2. 编辑配置文件，设置 API 密钥和语言

3. 从论文中提取术语
   /babeldoc glossary extract paper.pdf --output terms.csv

4. 编辑术语表，添加专业术语翻译

5. 使用术语表翻译论文
   /babeldoc translate paper.pdf --glossary-files terms.csv

6. 检查翻译结果
```

### 批量处理项目文档

```
1. 创建项目配置
   /babeldoc config init --template full

2. 批量翻译所有 PDF
   /babeldoc batch "./docs/*.pdf" --config ./babeldoc.toml

3. 检查输出目录中的翻译结果
```

## 翻译质量说明

### 理解翻译统计

翻译完成后，BabelDOC 会显示以下统计信息:

```
Translation completed. Total: 1093, Successful: 1031, Fallback: 62
```

- **Total**: 总段落数
- **Successful**: 成功通过 LLM 翻译的段落
- **Fallback**: 使用备用方法翻译的段落

正常情况下，Fallback 比例在 5-10% 是可接受的。这些 Fallback 通常是:
- 短文本 (如引用标记、单个字符)
- 人名、机构名等专有名词
- 数学公式、代码片段
- 样式标记 (如 `{v1}<style>`)

### 常见警告解释

翻译过程中可能出现以下 WARNING 日志:

| 警告类型 | 原因 | 影响 |
|---------|------|------|
| `Translation result is the same as input` | LLM 输出与原文相同 | 通常出现在非翻译内容 (人名、数字等) |
| `Edit distance is too small` | 译文与原文编辑距离过小 | 可能是短文本或格式标记 |
| `Translation result is too long or too short` | 输出长度异常 | 通常是单个字符或符号 |
| `Error Translation results length mismatch` | 段落数量不匹配 | 会自动重试或使用 Fallback |

**重要**: 这些警告是正常现象，BabelDOC 会自动使用 Fallback 方法处理，不会影响最终翻译质量。

### 翻译性能指标

根据实际测试 (38 页 NeurIPS 论文):

- **翻译时间**: 约 11 分钟 (681 秒)
- **成功翻译率**: 94.3%
- **平均速度**: 约 3.3 页/分钟
- **内存使用**: 峰值约 2.6 GB
- **Token 消耗**: 约 548k tokens (取决于模型)

## 故障排除

### 安装问题

**问题**: `pip install` 失败，提示依赖冲突

**解决方案**: 使用 `uv` 包管理器 (推荐):
```bash
pip install uv
uv tool install --python 3.12 BabelDOC
```

### 配置文件问题

**问题**: `Couldn't parse TOML file: 'gbk' codec can't decode byte`

**原因**: TOML 文件包含非 ASCII 字符 (如中文注释)

**解决方案**: 确保配置文件只使用英文注释，或删除所有注释:
```toml
[babeldoc]
debug = false
lang-in = "en-US"
lang-out = "zh-CN"
# 不要使用中文注释
```

### 翻译质量问题

**问题**: Fallback 比例过高 (>15%)

**可能原因和解决方案**:
1. 模型能力不足 - 换用更强的模型 (如 `gpt-4o` 而非 `mini`)
2. 源文档格式复杂 - 检查是否为扫描版，尝试 `--ocr-workaround`
3. 术语表不完整 - 使用 `/babeldoc glossary extract` 提取术语

### 性能优化建议

**提高翻译速度**:
```toml
# 增加并发数
qps = 10          # 每秒请求数 (谨慎调整，避免 API 限流)
pool-max-workers = 16  # 工作线程数
```

**降低 API 成本**:
- 使用 `glm-4-flash` 或 `deepseek-chat` 等性价比高的模型
- 避免重复翻译 (BabelDOC 有缓存机制)

**大文件处理**:
```toml
# 分块处理大文档
max-pages-per-part = 50
```

### PATH 配置

**问题**: `babeldoc: command not found`

**解决方案**: 将 uv 工具目录添加到 PATH:

Windows (PowerShell):
```powershell
$env:PATH = "C:\Users\$env:USERNAME\.local\bin;$env:PATH"
```

永久添加 (添加到系统环境变量):
```
C:\Users\<用户名>\.local\bin
```

## 常见问题

**Q: 如何设置自定义 OpenAI API 端点?**

A: 在配置文件中设置 `openai-base-url`，或在命令行使用 `--openai-base-url`。

**Q: 推荐使用哪些模型?**

A: 根据项目文档，推荐:
- `glm-4-flash` - 速度快 (智谱 AI)
- `deepseek-chat` - 性价比高 (DeepSeek)
- `gpt-4o-mini` - OpenAI 官方推荐

**Q: 翻译大文档时如何避免超时?**

A: 使用 `--max-pages-per-part` 选项将文档分块处理:
```
/babeldoc translate large_doc.pdf --max-pages-per-part 50
```

**Q: 如何处理扫描版 PDF?**

A: 启用 OCR 选项:
```
/babeldoc translate scanned.pdf --ocr-workaround
```

**Q: Fallback 翻译质量如何?**

A: Fallback 使用简单的逐词翻译，对于专业术语可能不准确。建议:
1. 使用术语表 (glossary) 提高专业词汇翻译质量
2. 检查 Fallback 段落，必要时手动修正
3. 换用更强的 LLM 模型减少 Fallback 比例

## 技术说明

### 环境要求

- Python 3.12+
- uv (推荐) 或 pip
- BabelDOC 已安装并可用

### API 兼容性

本技能使用 BabelDOC 命令行接口，支持任何 OpenAI 兼容的 API 端点。

### 已知限制

- 不支持线条渲染
- 大页面可能被跳过
- 作者和引用部分解析可能有误

## 参考资源

- [BabelDOC GitHub](https://github.com/funstory-ai/BabelDOC)
- [PDFMathTranslate (WebUI)](https://github.com/Byaidu/PDFMathTranslate)
- [沉浸式翻译 - BabelDOC](https://immersive-translate.owenchia.com/babeldoc)

## 贡献

欢迎提交问题和改进建议！

## 技能更新日志

### v1.0.0 (当前版本)

基于实际翻译经验 (38 页 NeurIPS 论文) 的改进：

**新增内容**:
- 翻译质量说明章节 (理解统计信息、常见警告、性能指标)
- 故障排除章节 (安装、配置、翻译质量、性能优化、PATH 配置)
- 文件路径详细说明 (输入/输出路径规则、推荐项目结构)
- Fallback 翻译质量说明

**修复问题**:
- 配置模板改为英文注释，避免 GBK 编码错误
- 添加 TOML 编码问题的解决方案

**性能参考**:
- 翻译速度: 约 3.3 页/分钟
- 成功翻译率: 94.3%
- Fallback 比例: 5.7% (正常范围)
- 内存使用: 峰值约 2.6 GB
