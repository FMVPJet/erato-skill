# Erato

> 以希腊抒情诗缪斯女神命名。

一个用于 Claude Code 的华语歌词创作 skill。支持普通话和粤语，三种创作模式，专业级质量检查，Suno/Udio 一键导出。

## 功能

| 模式 | 输入 | 输出 |
|---|---|---|
| **仿写** | 原曲 + 目标歌手 | 保留情感内核，用目标歌手风格重写 |
| **创作** | 歌手 + 主题 | 以该歌手的声音写一首新歌 |
| **原创** | 抽象风格 / 歌手混合 / 歌词片段 | 从风格描述出发写新歌 |

附加能力：
- **填词模式** — 按已有旋律的节奏/音高约束填词
- **粤语协音** — 九声标注 + 0-100 分协音评分
- **Suno/Udio 导出** — 自动生成风格标签和兼容格式

## 安装

软链到 Claude Code 的 skills 目录：

```bash
ln -s /path/to/erato-skill ~/.claude/skills/erato
```

或直接 clone：

```bash
git clone https://github.com/FMVPJet/erato-skill.git ~/.claude/skills/erato
```

## 使用

用中文自然提问即可：

- "让陈奕迅唱单依纯的《我表示理解》"
- "写一首周杰伦风格的歌，主题是父亲"
- "写一首忧郁的城市民谣"
- "帮我填词，每行 7 个字"
- "导出 Suno 格式"

## 目录结构

```
├── SKILL.md                        # 主 skill 定义（232 行）
├── test-cases.md                   # 10 个测试用例
└── references/                     # 按需加载的参考文件
    ├── style-extraction.md         # 六维度歌手风格归纳法
    ├── abstract-style-keywords.md  # 抽象描述 → 具体特征映射
    ├── song-structures.md          # 曲式模板（V-C-B 等）
    ├── song-metadata-db.md         # 知名歌曲结构元数据
    ├── rhyme-guide.md              # 普通话十三辙 + 粤语韵部
    ├── chorus-hook-techniques.md   # 副歌记忆点技巧
    ├── anti-ai-patterns.md         # 反 AI 味指南
    ├── singability.md              # 唱感（开口音/气口/长音位）
    ├── melody-first-mode.md        # 填词模式
    ├── cantonese-tones.md          # 粤语九声系统
    ├── cantonese-register.md       # 粤语书面 ↔ 口语频谱
    ├── tone-harmony-scoring.md     # 协音评分（0-100）
    ├── mode-prompts.md             # 各模式自检清单
    ├── suno-export.md              # Suno 导出 + 歌手→风格标签映射
    ├── web-search-strategy.md      # 歌词搜索策略
    └── artist-profiles/            # 歌手风格缓存（使用中积累）
```

## 核心特性

- **反 AI 味** — 主动避免过度对称、万能抒情词、泛化隐喻
- **唱感优化** — 副歌高潮位优先使用开口韵母
- **V2 差异化** — 第二段必须换角度（不是第一段的稀释版）
- **风格距离处理** — 原曲和目标风格差距大时，明确转换表达方式并保留情感本质
- **Hook 优先** — 先写副歌记忆点，再围绕它展开
- **粤语协音** — 0-100 评分，低于 75 分自动修复拗音

## 许可证

MIT
