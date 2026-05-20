# Suno/Udio Export Format

When the user wants to use the lyrics with AI music generation tools (Suno, Udio, etc.), output an additional export-ready version.

## Trigger

Activate when the user mentions:
- "Suno" / "Udio" / "AI 作曲" / "生成音乐"
- "导出" / "export" / "可以直接用的格式"
- Or when the user explicitly asks for a music-tool-compatible version

## Suno format

Suno uses section tags in square brackets. The standard output already uses compatible tags (`[Verse 1]`, `[Chorus]`, etc.), but Suno also supports:

### Additional tags
- `[Intro]` — instrumental intro
- `[Outro]` — instrumental outro
- `[Instrumental]` — instrumental break between sections
- `[Break]` — short pause/break
- `[Hook]` — short repeated hook
- `[Ad-lib]` — background vocals / ad-libs
- `[Fade Out]` — fade ending

### Style prompt line

Add a style prompt at the top (not part of lyrics, just for Suno's style input):

```
Style: soft ballad, male vocal, piano, emotional, Mandarin
```

Common style descriptors:
- Tempo: slow ballad / mid-tempo / upbeat / fast
- Vocal: male vocal / female vocal / duet
- Instrument: piano / guitar / strings / electronic / band
- Mood: emotional / uplifting / melancholic / energetic
- Language: Mandarin / Cantonese

### Artist → Suno Style mapping

When the lyrics were written for a specific artist, use this table to auto-generate the most fitting Suno style tag:

| Artist / Style | Recommended Suno Style |
|---|---|
| 陈奕迅 (粤语抒情) | `Cantonese ballad, male vocal, emotional, piano, strings, mid-tempo` |
| 陈奕迅 (粤语快歌) | `Cantonese pop, male vocal, upbeat, band, energetic` |
| 陈奕迅 (普通话) | `Mandarin pop ballad, male vocal, piano, emotional, mid-tempo` |
| 周杰伦 (中国风) | `Mandarin pop, male vocal, Chinese instruments, R&B, mid-tempo, cinematic` |
| 周杰伦 (抒情) | `Mandarin ballad, male vocal, piano, strings, emotional, slow` |
| 周杰伦 (快歌) | `Mandarin pop, male vocal, hip-hop, upbeat, electronic` |
| 林俊杰 | `Mandarin pop ballad, male vocal, piano, emotional, warm, mid-tempo` |
| 王菲 | `Mandarin/Cantonese dream pop, female vocal, ethereal, strings, atmospheric` |
| 李宗盛 | `Mandarin folk ballad, male vocal, acoustic guitar, spoken-word, slow, intimate` |
| 五月天 | `Mandarin rock, male vocal, band, energetic, uplifting, anthemic` |
| 邓紫棋 | `Mandarin pop, female vocal, powerful, electronic, piano, mid-tempo` |
| 薛之谦 | `Mandarin pop ballad, male vocal, piano, melancholic, mid-tempo` |
| 毛不易 | `Mandarin folk pop, male vocal, acoustic guitar, gentle, slow, intimate` |
| 房东的猫 | `Mandarin indie folk, female vocal, acoustic, warm, gentle, slow` |
| 忧郁民谣 | `Indie folk, acoustic guitar, melancholic, slow, intimate, lo-fi` |
| 城市流行 | `Mandarin pop, synth, mid-tempo, urban, modern, polished` |
| 复古港乐 | `Cantonese retro pop, 80s synth, nostalgic, mid-tempo, warm` |
| 摇滚 | `Chinese rock, electric guitar, drums, energetic, raw, fast` |
| 古风 | `Chinese traditional, guzheng, flute, ethereal, slow, cinematic` |

**Usage**: Match the target artist first. If no exact match, combine the closest artist style with the song's mood/tempo. If original mode with abstract style, use the bottom section of the table.

**Combining tags**: Suno works best with 5-8 descriptors. Pick: genre + vocal + 1-2 instruments + mood + tempo + language.

### Export output format

```
---
【Suno 导出版】

Style: {style descriptors}

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

## Udio format

Udio uses a similar tag system. The main differences:
- Udio prefers shorter sections (4 lines max per tag)
- Udio supports `[Verse]` without numbers
- Style is set separately in the UI, not in the lyrics text

## Rules

- Strip the metadata header (【模式】【语言】etc.) from the export version — Suno doesn't need it
- Keep section tags
- Add `[Instrumental]` between sections where a musical break feels natural (typically between Chorus and Verse 2, or before Bridge)
- Don't add `[Intro]` or `[Outro]` unless the song structure calls for it
- The export is an ADDITIONAL output — always show the standard format first, then the export version below it
