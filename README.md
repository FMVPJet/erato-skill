# Erato

> Named after the Greek muse of lyric poetry.

A Claude Code skill for writing Mandarin and Cantonese song lyrics. Three modes, professional-grade quality gates, and Suno/Udio export.

## What it does

| Mode | Input | Output |
|---|---|---|
| **Imitation 仿写** | Existing song + target artist | Same emotional core, new words in target style |
| **Creation 创作** | Artist + theme | New song in that artist's voice |
| **Original 原创** | Abstract style / artist mix / snippets | New song from style spec |

Plus:
- **Melody-First 填词** — fill words to existing rhythmic/melodic constraints
- **Cantonese tone-harmony** — 9-tone annotation + 0-100 scoring
- **Suno/Udio export** — auto-generates style tags and compatible format

## Installation

Symlink or copy into your Claude Code skills directory:

```bash
ln -s /path/to/erato-skill ~/.claude/skills/erato
```

Or clone directly:

```bash
git clone https://github.com/FMVPJet/erato-skill.git ~/.claude/skills/erato
```

## Usage

Just ask naturally in Chinese:

- "让陈奕迅唱单依纯的《我表示理解》"
- "写一首周杰伦风格的歌，主题是父亲"
- "写一首忧郁的城市民谣"
- "帮我填词，每行 7 个字"
- "导出 Suno 格式"

## Structure

```
├── SKILL.md                        # Main skill definition (232 lines)
├── test-cases.md                   # 10 test cases for validation
└── references/                     # Loaded on-demand by the skill
    ├── style-extraction.md         # 6-dimension artist profiling method
    ├── abstract-style-keywords.md  # Map abstract descriptors → features
    ├── song-structures.md          # Structure templates (V-C-B etc.)
    ├── song-metadata-db.md         # Known songs' structural metadata
    ├── rhyme-guide.md              # 十三辙 + Cantonese rhyme groups
    ├── chorus-hook-techniques.md   # Hook line craft
    ├── anti-ai-patterns.md         # Avoid AI-flavored output
    ├── singability.md              # Vowel openness + breath points
    ├── melody-first-mode.md        # Filling words to melody
    ├── cantonese-tones.md          # 9-tone system
    ├── cantonese-register.md       # Colloquial ↔ literary spectrum
    ├── tone-harmony-scoring.md     # 0-100 tone-melody clash scoring
    ├── mode-prompts.md             # Per-mode self-check checklists
    ├── suno-export.md              # Suno format + artist→style mapping
    ├── web-search-strategy.md      # Lyrics search escalation
    └── artist-profiles/            # Cached artist profiles (grows over use)
```

## Key features

- **Anti-AI patterns** — actively avoids over-symmetry, 万能抒情词, and generic metaphors
- **Singability** — prioritizes open vowels on held notes and chorus peaks
- **V2 differentiation** — Verse 2 must shift angle from Verse 1 (not a diluted repeat)
- **Style distance handling** — when source and target styles are far apart, explicitly transforms the expression mode while preserving emotional core
- **Hook-first writing** — chorus is built around the hook line, not the other way around
- **粤语 tone-harmony** — scores lyrics 0-100 and auto-fixes clashes below 75

## License

MIT
