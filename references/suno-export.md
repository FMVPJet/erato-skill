# Suno/Udio Export Format

When the user wants to use the lyrics with AI music generation tools (Suno, Udio, etc.), output an additional export-ready version.

## Trigger

Activate when the user mentions:
- "Suno" / "Udio" / "AI 作曲" / "生成音乐"
- "导出" / "export" / "可以直接用的格式"
- Or when the user explicitly asks for a music-tool-compatible version

**Passive trigger**: If the user did NOT mention Suno during the initial request, the iteration prompt at the end of lyrics output includes a hint: "需要 Suno/Udio 导出版可以告诉我。" This avoids forcing the user to re-explain context.

## Suno format

Suno uses section tags in square brackets. The standard output already uses compatible tags (`[Verse 1]`, `[Chorus]`, etc.), but Suno also supports:

### Additional tags
- `[Intro]` — instrumental intro
- `[Outro]` — instrumental outro
- `[Instrumental]` — instrumental break between sections
- `[Interlude]` — longer instrumental passage
- `[Break]` — short pause/break
- `[Hook]` — short repeated hook
- `[Ad-lib]` — background vocals / ad-libs
- `[Fade Out]` — fade ending

### Lyrics length guidance

Suno has practical limits on lyrics length:
- **Optimal**: 1200-2000 characters (Chinese) per generation
- **Maximum**: ~3000 characters before quality degrades
- If the lyrics exceed 2000 characters, add a note: `【提示】歌词较长（{n}字），建议在 Suno 中分段生成（先生成到 Bridge 前，再 Extend 续写）`
- Suggest a natural split point (usually after the first Chorus or before Bridge)

### Style prompt line

Add a style prompt at the top (not part of lyrics, just for Suno's style input):

```
Style: soft ballad, male vocal, piano, emotional, Mandarin
```

**Interactive confirmation**: Don't just output the style tag silently. Present it to the user for confirmation:

> 🎵 Suno Style 建议：`soft ballad, male vocal, piano, emotional, Mandarin`
> 
> 需要调整吗？比如加入某种乐器、改变速度、或换一种氛围。确认后我输出完整导出版。

Only skip confirmation when:
- The user explicitly provided style descriptors (e.g. "导出成 lo-fi 风格的 Suno 格式")
- The user said "直接导出" or similar indicating they don't want to review

**Language tag**: Always use the actual output language of the lyrics, not the artist's "typical" language. If lyrics are in 普通话, tag `Mandarin`; if 粤语, tag `Cantonese` — regardless of the artist mapping table.

Common style descriptors:
- Tempo: slow ballad / mid-tempo / upbeat / fast
- Vocal: male vocal / female vocal / duet
- Instrument: piano / guitar / strings / electronic / band / Chinese instruments
- Mood: emotional / uplifting / melancholic / energetic / dreamy / raw
- Language: Mandarin / Cantonese
- Texture: lo-fi / polished / acoustic / atmospheric / cinematic

### Artist → Suno Style mapping

When the lyrics were written for a specific artist, use this table as a **starting point** for the style tag (user may adjust):

| Artist / Style | Recommended Suno Style |
|---|---|
| 陈奕迅 (抒情) | `ballad, male vocal, emotional, piano, strings, mid-tempo` |
| 陈奕迅 (快歌) | `pop, male vocal, upbeat, band, energetic` |
| 周杰伦 (中国风) | `pop, male vocal, Chinese instruments, R&B, mid-tempo, cinematic` |
| 周杰伦 (抒情) | `ballad, male vocal, piano, strings, emotional, slow` |
| 周杰伦 (快歌) | `pop, male vocal, hip-hop, upbeat, electronic` |
| 林俊杰 | `pop ballad, male vocal, piano, emotional, warm, mid-tempo` |
| 王菲 | `dream pop, female vocal, ethereal, strings, atmospheric` |
| 李宗盛 | `folk ballad, male vocal, acoustic guitar, spoken-word, slow, intimate` |
| 五月天 | `rock, male vocal, band, energetic, uplifting, anthemic` |
| 邓紫棋 | `pop, female vocal, powerful, electronic, piano, mid-tempo` |
| 薛之谦 | `pop ballad, male vocal, piano, melancholic, mid-tempo` |
| 毛不易 | `folk pop, male vocal, acoustic guitar, gentle, slow, intimate` |
| 房东的猫 | `indie folk, female vocal, acoustic, warm, gentle, slow` |
| 忧郁民谣 | `indie folk, acoustic guitar, melancholic, slow, intimate, lo-fi` |
| 城市流行 | `synth pop, mid-tempo, urban, modern, polished` |
| 复古港乐 | `retro pop, 80s synth, nostalgic, mid-tempo, warm` |
| 摇滚 | `Chinese rock, electric guitar, drums, energetic, raw, fast` |
| 古风 | `Chinese traditional, guzheng, flute, ethereal, slow, cinematic` |

**Note**: Table entries no longer include language tags — language is determined by the actual lyrics output (see "Language tag" above).

**Usage**: Match the target artist first. If no exact match, combine the closest artist style with the song's mood/tempo. If original mode with abstract style, use the bottom section of the table.

**Combining tags**: Suno works best with 5-8 descriptors. Pick: genre + vocal + 1-2 instruments + mood + tempo + language (appended automatically).

### `[Instrumental]` insertion rules

Don't insert `[Instrumental]` based on gut feeling. Use the emotion curve to decide:

**Insert `[Instrumental]` when**:
- Between two sections where emotion intensity drops ≥2 points (cool-down moment)
- After the first Chorus and before Verse 2 (standard breathing space)
- Before Bridge (transition into the "third act")

**Don't insert when**:
- Emotion is building continuously (e.g. V1→Pre-Chorus→Chorus with no dip)
- Between Bridge and Final Chorus (momentum should carry through)
- The song is already short (≤8 lines total) — instrumental breaks dilute impact

**Example** (using emotion curve from the lyrics):
```
[V1] ▂▃▃▂ (intensity 3-4)
[C]  ▇▇█▇ (intensity 8)
[Instrumental]          ← drop from 8 to ~4, natural cool-down
[V2] ▃▄▄▃ (intensity 4-5)
[C]  ▇▇█▇ (intensity 8)
[Bridge] ▅▄▃▄ (intensity 5→3→5)
[C]  ▇██▇ (intensity 9, peak)  ← NO instrumental before this
```

### Export output format

```
---
【Suno 导出版】

Style: {style descriptors}, {language}

[Intro]
[Verse 1]
歌词...

[Pre-Chorus]
歌词...

[Chorus]
歌词...

[Instrumental]

[Verse 2]
歌词...

[Chorus]
歌词...

[Bridge]
歌词...

[Chorus]
歌词...

[Outro]
---
```

### Export workflow (step by step)

1. **Count characters** — if total lyrics > 2000 chars, add length warning and suggest split point
2. **Generate style tag** — from artist mapping + actual output language
3. **Present style tag for confirmation** — unless user said "直接导出"
4. **Wait for user OK or adjustment**
5. **Output export version** — with `[Instrumental]` placed per emotion curve rules
6. **If split needed** — mark the split point with `--- 建议在此处分段生成 ---`

## Udio format

Udio uses a similar tag system. The main differences:
- Udio prefers shorter sections (4 lines max per tag)
- Udio supports `[Verse]` without numbers
- Style is set separately in the UI, not in the lyrics text
- When exporting for Udio, still output the style tag as a reference for the user to paste into Udio's style field

## Rules

- Strip the metadata header (【模式】【语言】etc.) from the export version — Suno doesn't need it
- Keep section tags
- Place `[Instrumental]` based on emotion curve cool-down points (see rules above)
- Don't add `[Intro]` or `[Outro]` unless the song structure calls for it
- The export is an ADDITIONAL output — always show the standard format first, then the export version below it
- Language in style tag must match the actual lyrics language, not the artist's "default" language
