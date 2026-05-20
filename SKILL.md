---
name: erato
version: 1.2.1
description: Write Mandarin or Cantonese song lyrics in three modes — imitation (rewrite an existing song in another artist's voice), creation (write for a given artist and theme), or original (write from scratch given style references). Named after Erato, the Greek muse of lyric poetry. Use when the user asks to 写歌词 / 作词 / 仿写一首歌 / 让某歌手风格唱另一首 / 创作一首XX风格的歌 / 原创一首歌词. Skip for pure lyrics translation or analysis.
---

# Lyrics Writing

## Quick Start (for Claude)

When this skill is invoked:

1. **First**: Determine the mode (imitation / creation / original / melody-first)
2. **Then**: Load the corresponding reference files as you go (don't load all at once)
3. **Write**: Follow the mode's workflow step-by-step
4. **Check**: Run quality gates after writing
5. **Output**: Format according to the output template

**Key principle**: Load references on-demand, not upfront. This keeps token usage efficient.

## Overview

Generate Mandarin or Cantonese song lyrics in one of three modes: **仿写 (imitation)**, **创作 (creation)**, or **原创 (original)**. Cantonese output supports an optional **协音 (tone-harmony)** annotation mode. The skill produces only the lyrics text — not melody, arrangement, or full lyric analysis.

## Mode Routing

Decide the mode from the user's request:

| User intent | Mode | Example |
|---|---|---|
| Existing song + a different artist | **imitation 仿写** | "让陈奕迅唱单依纯的《珠玉》" |
| An artist + a theme/emotion/story (no source song) | **creation 创作** | "写一首陈奕迅风格的失恋歌" |
| No specific artist anchor; only theme/abstract style/snippets | **original 原创** | "写一首忧郁的城市民谣" |

### Ambiguity

If the request mixes signals (e.g. "写一首陈奕迅风格、像《富士山下》那样的歌"), ask one clarifying question:

> 你是想把《富士山下》改写成另一首意境相近的歌（仿写），还是用陈奕迅 +《富士山下》的语感重新创作一首新主题？

### Explicit parameters (user can override)

- **Language**: 普通话 / 粤语 (default: infer from request, ask if unclear)
- **Tone-harmony mode**: on / off (Cantonese only, default off)
- **Structure**: auto / user-specified (e.g. `verse-chorus-verse-chorus-bridge-chorus`)
- **Suno export**: on / off (default off; set to on if user mentions Suno/Udio/AI music anywhere in the request)

### Do NOT trigger for

- Lyric translation
- Lyric interpretation/analysis
- Composing melodies, arrangement, or harmony (but DO accept filling words to an existing melody — see Mode 0)
- Lyric copyright/legal questions

---

## Workflow

### Mode 0: Melody-First (填词模式)

Activated when the user provides **explicit rhythmic or melodic constraints**. Load `references/melody-first-mode.md` for full details.

**Trigger**: user provides ANY of:
- Per-line character counts (e.g. "7-7-5-7")
- Stress patterns (e.g. "x-X-x-X")
- Pitch contours (e.g. "高-中-低-高")
- Reference melody (e.g. "按《富士山下》的旋律填词")

**Ambiguous trigger**: If user says "帮我填词" / "按旋律写词" but provides NO constraints:
- Ask: "你有具体的旋律约束吗？比如每行字数、节奏重音、或音高走向？如果没有，我就按常规模式写词。"
- Wait for user response before proceeding

This mode combines with any of the three main modes (imitation + melody-first, creation + melody-first, original + melody-first). The melody constraints become hard requirements; **character count must match exactly (±0), overriding the ±2 tolerance in imitation mode**.

### Mode 1: Imitation (仿写)

Inputs: source song lyrics + target artist + language.

1. **Get the source lyrics.** Prefer what the user provided. Otherwise recall from model memory; if unsure, **trigger WebSearch** following `references/web-search-strategy.md` escalation strategy (up to 3 attempts with different query formulations). If still unavailable, ask the user for the lyrics.
2. **Distill the emotional core and narrative skeleton.** Write one sentence for the emotional core (*what the song is really about beneath the literal story*, e.g. 《珠玉》→「珍视一段易碎而珍贵的关系/记忆」), then 2–3 sentences for the narrative skeleton (key turning points or progression). Capture the emotional arc per section.
3. **Capture the structural skeleton** — number of sections, lines per section, **characters per line**, rhyme density, where the chorus sits. Record the per-line character count — the new lyrics should match within ±2 characters per line.
4. **Profile the target artist's voice.** Load `references/style-extraction.md` and apply the six-dimension method (seven dimensions for singers: add vocal characteristics). For artists you don't know well (≤2 representative songs recallable, no specific stylistic features), **trigger WebSearch** following `references/web-search-strategy.md` escalation strategy (up to 2 attempts for artist profiling). If still insufficient, ask the user for 2-3 representative lyrics.
5. **Design the emotion curve.** Load `references/emotion-dynamics.md` and analyze the source song's emotion curve (mark intensity 1-10 for each section). The new lyrics should follow the same curve shape.
6. **Rewrite.** Tell the *same emotional core* using the target artist's vocabulary, viewpoint, and imagery, following the original's structural skeleton and emotional arc. Load `references/imagery-library.md` — choose imagery that fits the target artist's style and the emotion of each section (at least 2 concrete images per verse). Load `references/rhyme-guide.md` for rhyme strategy (match original's rhyme density and turning points; consider rhyme emotional color). Load `references/chorus-hook-techniques.md` — identify the original's hook position and write a new hook of equal weight in the same position. Load `references/lyric-rhythm.md` — ensure hook line is the shortest rhythmic unit and rhythm matches emotion intensity. Load `references/anti-ai-patterns.md` while writing — avoid over-symmetry and 万能抒情词 as you go. Match the original's per-line character count within ±2 characters. **Verse 2 must differ from Verse 1** — shift the angle (different time, different viewpoint, deeper layer, or consequence of V1's situation). **Final Chorus may vary** from earlier choruses — add a tag line, change 1-2 words to intensify emotion, or extend by one line.
   
   **When style distance is large** (e.g. cute→philosophical, rock→folk): preserve the emotional core's *essence* (joy/sadness/release) but allow the *expression mode* to shift (naive joy → knowing joy, raw anger → quiet defiance). State the transformation clearly in the output's `【关键转换】` field.
7. **Do NOT preserve the source's literal narrative or specific imagery.** A direct line transplant feels forced — replace surface details with ones natural to the target artist.

### Mode 2: Creation (创作)

Inputs: target artist + theme/emotion/story + language.

1. **Align on the theme.** If the user is vague (only "失恋"), you may ask one anchoring question (which stage? whose viewpoint?). Don't force it — for terse requests, take the most common reading.
2. **Profile the target artist's voice.** Load `references/style-extraction.md` and apply the seven-dimension method (including vocal characteristics for singers).
3. **Check theme/artist fit.** If clearly mismatched (a children's-song artist + a dark theme, etc.), surface the conflict and offer two options (swap artist / swap theme). Proceed with whatever the user decides.
4. **Pick a structure.** Load `references/song-structures.md` and choose one suited to the theme and emotion.
5. **Design the emotion curve.** Load `references/emotion-dynamics.md` and choose a curve type that fits the theme. Ensure V1-C intensity gap ≥3, and Final Chorus is the peak.
6. **Write.** Use the artist's typical vocabulary, viewpoint, and imagery to write a new song on the given theme. Load `references/imagery-library.md` — choose imagery that fits the artist's style (at least 2 concrete images per verse). Load `references/rhyme-guide.md` for rhyme strategy (consider rhyme emotional color: open rhymes for chorus climax, closed rhymes for introspective verses). Load `references/chorus-hook-techniques.md` and apply hook techniques to the chorus — write the hook line first, then build the rest of the chorus around it. Load `references/lyric-rhythm.md` — ensure hook line is the shortest rhythmic unit, vary phrasing patterns, and match rhythm to emotion intensity. Load `references/anti-ai-patterns.md` while writing. **Verse 2 must differ from Verse 1** — shift the angle (different time, different viewpoint, deeper layer, or consequence of V1's situation). **Final Chorus may vary** — add a tag line, change 1-2 words to intensify, or extend by one line.

### Mode 3: Original (原创)

Inputs: theme + style specification (one of: abstract description / artist mix / lyric snippets) + language.

1. **Parse the style spec:**
   - **Abstract description** (e.g. "忧郁的城市民谣"): load `references/abstract-style-keywords.md` to map abstract descriptors to concrete lyrical features (vocabulary, imagery, structure, tone).
   - **Artist mix** (e.g. "林夕 + 方文山"): load `references/style-extraction.md`, profile each artist, find the intersection (shared traits) and union (combined palette).
   - **Lyric snippets**: derive vocabulary tendencies, syntax patterns, and rhetorical density from the snippets.
2. **Pick a structure.** Load `references/song-structures.md`.
3. **Design the emotion curve.** Load `references/emotion-dynamics.md` and choose a curve type based on the style spec (e.g. "忧郁的民谣" → plateau or wave type; "爆发力的摇滚" → build-up type). Ensure V1-C intensity gap ≥3.
4. **Write.** Load `references/imagery-library.md` — choose imagery that fits the style spec (at least 2 concrete images per verse; use the pairing principles for fresh combinations). Load `references/rhyme-guide.md` for rhyme strategy (consider rhyme emotional color to match the mood). Load `references/chorus-hook-techniques.md` and apply hook techniques — write the hook line first, then build the rest of the chorus around it. Load `references/lyric-rhythm.md` — ensure hook line is the shortest rhythmic unit, vary phrasing patterns, and match rhythm to emotion intensity. Load `references/anti-ai-patterns.md` while writing. **Verse 2 must differ from Verse 1** — shift the angle (different time, different viewpoint, deeper layer, or consequence of V1's situation). **Final Chorus may vary** — add a tag line, change 1-2 words to intensify, or extend by one line.

### After writing: quality gates (all modes)

Run these checks in order after the draft is complete:

- **Singability check.** Load `references/singability.md`. Verify chorus peak lines end with open vowels; verify long lines have breath points; flag closed vowels on emotional climax positions.
- **Tone-harmony annotation** (only when language is 粤语 *and* the user enabled the mode). Two scenarios:
  
  **Scenario A: User provided melody constraints** (pitch contours via melody-first mode):
  - Prioritize tone-matched characters *while writing* — choose characters whose tone bucket (high/mid/low) aligns with the melody pitch at that position
  - Load `references/cantonese-tones.md` and append the 1–9 tone marks to each line
  - Load `references/tone-harmony-scoring.md` and run the scoring system — if score < 75, fix flagged positions before presenting to user
  
  **Scenario B: User did NOT provide melody** (tone-harmony mode without melody-first):
  - Write naturally first, then annotate with tone marks
  - Load `references/cantonese-tones.md` and append the 1–9 tone marks to each line
  - Do NOT attempt to "optimize" tone placement without melody information
  - Output includes tone marks for reference, but no scoring (scoring requires melody)
  - Add a note in output: `【协音说明】已标注声调，如需优化协音请提供旋律音高信息`
- **Self-check.** Load `references/mode-prompts.md` and run the checklist for the mode just executed. Revise if any item fails.
- **Copyright check** (imitation and creation modes only). Scan the output for:
  - **Imitation mode**: iconic lines or signature imagery from the source song (e.g. "富士山下" if imitating 《富士山下》, "红玫瑰/白玫瑰" if imitating 《红玫瑰》)
  - **Creation mode**: iconic lines from the target artist's catalog (e.g. don't write "天青色等烟雨" in a 方文山-style song, don't write "如果你太累" in an 陈奕迅-style song)
  - If found, replace with original wording that conveys the same meaning/emotion
- **Emotion curve** (only on first full output, not on partial edits). Append a text-based emotion curve:
    ```
    [V1] ▂▃▃▂  描述
    [C]  ▇▇█▇  描述
    ```
- **Song title suggestion.** Suggest 1-2 candidate song titles based on the hook line or central image. Format: `【建议歌名】A / B`
- **Format the output.** See "Output Format" below.
- **Suno export** (only when user requests or mentions Suno/Udio/AI music). Load `references/suno-export.md` and follow the 8-step export workflow: (1) count characters, (2) generate Global Style Prompt (detailed 4-element English paragraph), (3) present for user confirmation, (4) wait for OK/adjustment, (5) generate Sectional Prompts (map emotion curve to arrangement density + song progression logic), (6) insert `[Instrumental]` breaks per emotion curve rules, (7) generate parameter recommendations (Vocal Gender / Weirdness / Style Influence), (8) output export version with usage instructions.
- **Iteration prompt.** End with: "如需修改某段或某句，告诉我具体位置和方向。" If Suno export was NOT triggered in this session, append: "需要 Suno/Udio 导出版可以告诉我。"

### User requests modifications

When the user asks to revise the output:

- **局部改词** (specific line/section changes): edit only the requested part, keep everything else unchanged.
- **改情绪/风格** (change emotion or style): re-run the corresponding mode's workflow from step 1.
- **改结构** (change structure): treat as a new task and restart from mode routing.

### Hook candidates mode

When the user asks for options, or when you're unsure which direction the chorus should take, present 2-3 hook line candidates before writing the full chorus:

> 副歌 hook 候选：
> 1. "原来你的背影是一封信" — 矛盾感（背影 vs 信）
> 2. "来不及是最诚实的情书" — 抽象概念拟物
> 3. "我在这里像一棵老樟树" — 具象比喻
>
> 你倾向哪个方向？选定后我围绕它展开副歌。

If the user doesn't ask for options, pick the strongest hook and proceed directly.

### Multi-variant mode

When the user says `--variants` or "给我两个版本" or "出几版对比", output 2 versions of the full lyrics with different approaches:

- **Version A**: more faithful to the emotional core / target artist's typical style
- **Version B**: more experimental / unexpected angle on the same theme

Label each version clearly and let the user pick or mix.

---

## Output Format

### Standard

```
【模式】仿写 / 创作 / 原创
【语言】普通话 / 粤语
【目标歌手】陈奕迅                # imitation, creation only
【原曲】单依纯《珠玉》            # imitation only
【主题/情感内核】一句话概括
【关键转换】原曲气质→目标气质     # only when style distance is large
【结构】Verse 1 - Chorus - Verse 2 - Chorus - Bridge - Chorus
【建议歌名】A / B                 # 1-2 candidates based on hook/central image

---

[Verse 1]
歌词第一行
歌词第二行
…

[Chorus]
歌词…

…

---

如需修改某段或某句，告诉我具体位置和方向。
```

### Tone-harmony variant (Cantonese only)

Each line is followed by space-separated tone numbers, one per character, in brackets:

```
[Verse 1]
歌词第一行    [4 3 2 1 5]
歌词第二行    [3 4 1 2 6]
```

Tone numbers use the 9-tone system: `1` 阴平 / `2` 阴上 / `3` 阴去 / `4` 阳平 / `5` 阳上 / `6` 阳去 / `7` 阴入 / `8` 中入 / `9` 阳入. See `references/cantonese-tones.md`.

### Section labels

Use English labels: `[Intro]`, `[Verse 1]`, `[Verse 2]`, `[Pre-Chorus]`, `[Chorus]`, `[Bridge]`, `[Outro]`.

### Don't include

Don't append rhyme analysis, rhetoric breakdowns, or "creation notes" unless the user asks. Output is the lyrics, not a teardown.

---

## Boundaries and Risks

### Copyright and originality

- **Don't reproduce others' full lyrics.** Lyrics found via `WebSearch` are for style profiling only — never quote whole stanzas or even memorable fragments (choruses, hooks, signature lines) in the output.
- **Imitation produces new lyrics.** Preserve the emotional core and structure; the wording must be newly written.
- **Style imitation ≠ lifting signature lines.** Don't pull an artist's iconic phrases verbatim (e.g. "红玫瑰" / "白玫瑰" stay out of an Eason-style imitation; they're tied to specific songs).

### Unfamiliar artists

- First try memory.
- Uncertain (≤2 representative songs recallable, no specific stylistic features) → load `references/web-search-strategy.md` and follow the escalation strategy.
- Search yields little → ask the user for 2–3 representative lyrics. Don't fabricate.

### Theme/artist conflict

- Clearly mismatched pairs → surface the conflict and offer alternatives, then proceed with the user's decision.

### Web search bounds

- Only when the model is uncertain or the user explicitly requests it. Not by default.
- Search fails → fall back to asking the user.

---

## Resources

- `references/style-extraction.md` — seven-dimension method (six lyrical + vocal characteristics) + lyricist quick-reference. Loaded by all three modes.
- `references/abstract-style-keywords.md` — maps abstract style descriptors to concrete lyrical features. Loaded by original mode (abstract branch).
- `references/song-structures.md` — structure templates and selection guidance. Loaded by creation and original modes.
- `references/song-metadata-db.md` — lightweight metadata database of well-known songs (structure, line counts, emotional arcs). Loaded by imitation mode for quick structural lookup, and by creation mode for reference.
- `references/emotion-dynamics.md` — emotion intensity curves (build-up, wave, flashback, plateau, burst-fade) and per-section intensity design. Loaded before writing in all modes.
- `references/imagery-library.md` — curated imagery organized by emotion, pairing principles, and artist-specific imagery preferences. Loaded during writing in all modes.
- `references/lyric-rhythm.md` — phrasing patterns, rhythm-emotion mapping, hook line rhythm design, and breath point rules. Loaded during writing in all modes.
- `references/rhyme-guide.md` — 普通话十三辙 + 粤语韵部 + 押韵策略 + 韵母情感色彩. Loaded during writing in all modes.
- `references/chorus-hook-techniques.md` — hook line techniques. Loaded during writing in all modes.
- `references/anti-ai-patterns.md` — patterns to avoid (over-symmetry, 万能抒情词, missing surprise) + 画面感自检. Loaded during writing in all modes.
- `references/singability.md` — vowel openness, breath points, held-note guidance. Loaded during singability check.
- `references/melody-first-mode.md` — constraints and workflow for filling words to an existing melody. Loaded when user provides rhythmic/melodic constraints.
- `references/mode-prompts.md` — per-mode self-check checklists. Loaded during self-check quality gate.
- `references/cantonese-tones.md` — 9-tone system, harmony principles, annotation method. Loaded when Cantonese tone-harmony mode is on.
- `references/tone-harmony-scoring.md` — scoring system for tone-melody clashes. Loaded after annotation.
- `references/suno-export.md` — export format + artist→style tag mapping for Suno/Udio. Loaded when Suno export is on.
- `references/web-search-strategy.md` — escalation strategy for finding lyrics and artist info via WebSearch. Loaded when search is needed.
- `references/cantonese-register.md` — guidance on written vs. colloquial Cantonese spectrum. Loaded when writing Cantonese lyrics.
- `references/artist-profiles/` — saved artist profiles for reuse across sessions.
