# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Erato is a Claude Code skill for professional-grade Mandarin and Cantonese song lyrics writing. Named after the Greek muse of lyric poetry, it supports three creative modes (imitation, creation, original) with quality gates for singability, tone-harmony, and anti-AI patterns.

## Architecture

### Core Components

**SKILL.md** (232 lines) — Main skill definition with mode routing, workflow orchestration, and quality gates. This is the entry point that Claude Code loads when the skill is invoked.

**references/** — Lazy-loaded reference files that provide domain expertise:
- Style analysis: `style-extraction.md` (6-dimension artist profiling), `abstract-style-keywords.md` (abstract→concrete mapping)
- Writing craft: `rhyme-guide.md` (普通话十三辙 + 粤语韵部), `chorus-hook-techniques.md`, `anti-ai-patterns.md`, `singability.md`
- Structure: `song-structures.md` (V-C-B templates), `song-metadata-db.md` (known songs' structural metadata)
- Cantonese: `cantonese-tones.md` (9-tone system), `cantonese-register.md` (written↔colloquial spectrum), `tone-harmony-scoring.md` (0-100 协音 scoring)
- Special modes: `melody-first-mode.md` (填词 with rhythmic constraints), `suno-export.md` (AI music platform export)
- Discovery: `web-search-strategy.md` (escalation strategy for unfamiliar artists/songs)
- `artist-profiles/` — Cached artist profiles built up across sessions

**test-cases.md** — 10 test scenarios covering all three modes, both languages, edge cases (unfamiliar artists, theme/artist conflicts, copyright boundaries, ambiguous requests).

### Mode Routing Logic

The skill routes user requests to one of three modes based on input signals:

| Input pattern | Mode | Example |
|---|---|---|
| Existing song + different artist | **Imitation 仿写** | "让陈奕迅唱单依纯的《珠玉》" |
| Artist + theme (no source song) | **Creation 创作** | "写一首陈奕迅风格的失恋歌" |
| Theme + abstract style / artist mix / snippets | **Original 原创** | "写一首忧郁的城市民谣" |

Ambiguous requests (e.g. "写一首陈奕迅风格、像《富士山下》那样的歌") trigger a clarifying question before routing.

### Quality Gates (Post-Writing)

All modes run these checks after the draft is complete:

1. **Singability check** — Verify chorus peaks use open vowels, long lines have breath points (load `singability.md`)
2. **Tone-harmony annotation** (Cantonese only, when enabled) — Append 1-9 tone marks, score 0-100, fix if <75 (load `cantonese-tones.md`, `tone-harmony-scoring.md`)
3. **Self-check** — Run mode-specific checklist (load `mode-prompts.md`)
4. **Emotion curve** — Text-based visualization (first output only)
5. **Song title suggestion** — 1-2 candidates based on hook/central image
6. **Suno export** (when requested) — Export-ready format with style tags (load `suno-export.md`)

### Key Design Principles

**Anti-AI patterns** — Actively avoid over-symmetry, 万能抒情词 (generic emotional words), and cliché metaphors. Load `anti-ai-patterns.md` during writing.

**V2 differentiation** — Verse 2 must shift angle (different time/viewpoint/layer), not dilute Verse 1.

**Hook-first writing** — Write the chorus hook line first, then build around it (load `chorus-hook-techniques.md`).

**Style distance handling** — When original and target styles are far apart (e.g. cute→philosophical), preserve emotional essence but allow expression mode to shift. Document the transformation in output's `【关键转换】` field.

**Copyright boundaries** — Never reproduce full lyrics or memorable fragments (choruses, hooks, signature lines) from existing songs. Imitation mode rewrites completely; source lyrics are for structure/emotion analysis only.

## Development Workflow

### Testing the Skill

Run test cases from `test-cases.md`:

```bash
# Test cases 1-5: core functionality (should pass 100%)
# Test cases 6-10: edge cases and robustness
```

**Test Case 5** (Cantonese tone-harmony) requires a Cantonese speaker to verify tone annotations.

**Test Case 6** (unfamiliar artists) tests whether the skill correctly triggers `WebSearch` when uncertain about an artist's style.

### Modifying Reference Files

When editing reference files:
- Keep them focused and token-efficient (they're loaded on-demand during writing)
- Use concrete examples over abstract principles
- Update `SKILL.md` if you add/remove/rename a reference file

### Adding New Artists

Artist profiles can be cached in `references/artist-profiles/` for reuse. Format:

```markdown
# [Artist Name]

## Six-Dimension Profile
- Vocabulary: ...
- Imagery: ...
- Viewpoint: ...
- Syntax: ...
- Rhetoric: ...
- Emotional range: ...

## Representative works
- Song 1 (year) — key features
- Song 2 (year) — key features
```

### Handling Unfamiliar Artists

The skill follows an escalation strategy (defined in `web-search-strategy.md`):
1. Try model memory first
2. If uncertain (≤2 representative songs recallable, no specific stylistic features), trigger `WebSearch` with escalating query formulations (up to 3 attempts)
3. If search fails, ask the user for 2-3 representative lyrics
4. Never fabricate artist styles

## Language and Localization

- The skill operates in Chinese (user requests and output are in 中文)
- Supports both 普通话 (Mandarin) and 粤语 (Cantonese) output
- Section labels in output use English: `[Verse 1]`, `[Chorus]`, `[Bridge]`, etc.
- Cantonese tone-harmony mode uses the 9-tone system: 1-阴平, 2-阴上, 3-阴去, 4-阳平, 5-阳上, 6-阳去, 7-阴入, 8-中入, 9-阳入

## Special Modes

**Melody-First (填词模式)** — Activated when user provides rhythmic/melodic constraints (per-line character counts, stress patterns, pitch contours). Load `melody-first-mode.md`. Character count must match exactly (±0), overriding the ±2 tolerance in imitation mode.

**Multi-Variant Mode** — When user says `--variants` or "给我两个版本", output 2 versions with different approaches (Version A: faithful, Version B: experimental).

**Hook Candidates Mode** — When uncertain about chorus direction, present 2-3 hook line candidates before writing the full chorus.

## Installation and Usage

This skill is designed to be symlinked into `~/.claude/skills/erato`:

```bash
ln -s /path/to/erato-skill ~/.claude/skills/erato
```

Trigger phrases (in Chinese):
- 写歌词 / 作词
- 仿写一首歌
- 让某歌手风格唱另一首
- 创作一首XX风格的歌
- 原创一首歌词

The skill does NOT trigger for:
- Pure lyrics translation
- Lyrics interpretation/analysis
- Melody/arrangement composition (but DOES accept 填词 — filling words to existing melody)
