# Suno/Udio Export Format

When the user wants to use the lyrics with AI music generation tools (Suno, Udio, etc.), output an additional export-ready version.

## Trigger

Activate when the user mentions:
- "Suno" / "Udio" / "AI 作曲" / "生成音乐"
- "导出" / "export" / "可以直接用的格式"
- Or when the user explicitly asks for a music-tool-compatible version

**Passive trigger**: If the user did NOT mention Suno during the initial request, the iteration prompt at the end of lyrics output includes a hint: "需要 Suno/Udio 导出版可以告诉我。" This avoids forcing the user to re-explain context.

## Suno V5 Format Overview

Suno V5 requires two parts:

1. **Global Style Prompt** — detailed English paragraph describing the artist's musical characteristics (4 core elements)
2. **Sectional Prompts + Lyrics** — each lyric section has a bracketed instruction controlling arrangement/emotion for that section

### Part 1: Global Style Prompt (全局风格提示词)

This is a detailed English paragraph (NOT a tag list) covering 4 core elements:

**1. Core Genre** — the primary genre(s) and fusion style (e.g. "Neo-Soul R&B", "Mandopop fusion", "Alternative Rock")

**2. Signature Instrumentation** — specific instruments and arrangement patterns (e.g. "piano-driven", "slap bass", "atmospheric synths", "string arrangements")

**3. Vocal Timbre & Technique** — voice characteristics and singing style (e.g. "breathy falsetto", "forceful shout-singing", "emotive vibrato", "spoken-word delivery")

**4. Production & Rhythmic Feel** — production style and groove (e.g. "cinematic production", "behind-the-beat groove", "minimalist and atmospheric", "polished pop production")

**Example** (JJ Lin style):
```
JJ Lin-style Mandopop/C-Pop, blending Pop-Rock with R&B and Ballad influences. The sound is defined by a highly melodic, piano-driven foundation, often accompanied by lush string arrangements and modern synth pads. His vocal style is a clear, powerful high tenor, known for its exceptional control, wide range, and signature emotive vibrato. Focus on polished, cinematic production and delivering a soaring, emotionally charged vocal performance.
```

**Language tag**: Always append the actual lyrics language at the end: `, Mandarin` or `, Cantonese`. This is determined by the lyrics output, NOT the artist's "typical" language.

#### How to generate the Global Style Prompt

**Step 1**: Load the artist profile from `style-extraction.md` (7 dimensions for singers: vocabulary, viewpoint, rhetoric, imagery, syntax, emotion, **vocal characteristics**)

**Step 2**: Map the 7 dimensions to Suno's 4 musical elements:

| Erato dimension | Maps to Suno element |
|---|---|
| Vocal characteristics (音域/速度/转音/气口/咬字) | → Vocal Timbre & Technique |
| Emotional register + typical tempo | → Core Genre + Rhythmic Feel |
| Rhetorical density + imagery style | → Production style (cinematic/raw/polished) |
| Sentence patterns (long/short lines) | → Rhythmic Feel (sustained/staccato) |

**Step 3**: Write a 3-5 sentence English paragraph covering all 4 elements. Use concrete musical terms, not vague adjectives.

**Step 4**: Append `, Mandarin` or `, Cantonese` based on the actual lyrics language.

#### Artist → Global Style Prompt Templates

Use these as starting points, then adapt based on the specific song's mood/tempo:

**陈奕迅 (Eason Chan) — Ballad**
```
Eason Chan-style Cantopop/Mandopop ballad, blending emotional pop with R&B influences. The sound is defined by lush orchestral arrangements with piano and string sections, often building to cinematic climaxes. His vocal style is a powerful tenor with exceptional emotional range, known for sustained notes, subtle vibrato, and the ability to convey deep vulnerability and restraint. Focus on polished production with dynamic builds, delivering a heartfelt, introspective vocal performance.
```

**陈奕迅 (Eason Chan) — Upbeat**
```
Eason Chan-style upbeat Cantopop/Mandopop, blending pop-rock with funk and electronic elements. The sound features driving drum beats, electric guitar riffs, and layered synths. His vocal delivery is energetic and rhythmic, with clear articulation and playful phrasing. Focus on tight, polished production with a groove-driven feel and dynamic vocal performance.
```

**周杰伦 (Jay Chou) — Chinese Style**
```
Jay Chou-style Mandopop with traditional Chinese influences, blending R&B with classical Chinese instrumentation. The sound features guzheng, pipa, erhu, and flute layered over modern hip-hop beats and piano. His vocal style is a mid-range tenor with rapid-fire delivery, mumbled storytelling flow, and signature rhythmic phrasing. Focus on cinematic, atmospheric production with a fusion of ancient and modern elements.
```

**周杰伦 (Jay Chou) — Ballad**
```
Jay Chou-style Mandopop ballad, piano-driven with lush string arrangements and subtle electronic textures. His vocal delivery is soft and intimate, with a breathy quality and emotional restraint. Focus on polished, cinematic production with sustained notes and delicate phrasing.
```

**周杰伦 (Jay Chou) — Fast/Hip-Hop**
```
Jay Chou-style Mandopop with hip-hop influences, featuring fast rap verses, electronic beats, and synthesizer layers. His vocal style is characterized by rapid articulation, rhythmic flow, and playful wordplay. Focus on upbeat, energetic production with punchy beats and dynamic vocal delivery.
```

**林俊杰 (JJ Lin)**
```
JJ Lin-style Mandopop/C-Pop, blending Pop-Rock with R&B and Ballad influences. The sound is defined by a highly melodic, piano-driven foundation, often accompanied by lush string arrangements and modern synth pads. His vocal style is a clear, powerful high tenor, known for its exceptional control, wide range, and signature emotive vibrato. Focus on polished, cinematic production and delivering a soaring, emotionally charged vocal performance.
```

**王菲 (Faye Wong)**
```
Faye Wong-style dream pop/alternative Cantopop, blending ethereal vocals with atmospheric production. The sound features delicate string arrangements, ambient synths, and minimalist instrumentation. Her vocal style is a breathy, crystalline soprano with a detached, otherworldly quality and subtle vibrato. Focus on spacious, atmospheric production with a floating, dreamlike feel.
```

**李宗盛 (Jonathan Lee)**
```
Jonathan Lee-style folk ballad, blending acoustic storytelling with intimate production. The sound is defined by acoustic guitar as the primary instrument, with minimal accompaniment and organic textures. His vocal style is a warm, conversational mid-range with a spoken-word quality, clear articulation, and emotional directness. Focus on stripped-down, intimate production with a narrative, confessional feel.
```

**五月天 (Mayday)**
```
Mayday-style Chinese rock, blending anthemic pop-rock with uplifting melodies and driving energy. The sound features full band arrangements with electric guitars, powerful drums, and soaring vocal harmonies. The vocal style is a passionate, mid-range tenor with clear articulation and an uplifting, communal quality. Focus on polished, arena-ready production with dynamic builds and sing-along choruses.
```

**邓紫棋 (G.E.M.)**
```
G.E.M.-style Cantopop/Mandopop, blending powerful pop with R&B and electronic influences. The sound features piano, electronic beats, and lush string arrangements. Her vocal style is a powerful soprano with exceptional range, signature vocal runs, and explosive high notes. Focus on polished, modern production with dynamic vocal showcases and emotional intensity.
```

**薛之谦 (Joker Xue)**
```
Joker Xue-style Mandopop ballad, blending melancholic pop with piano-driven arrangements. The sound features piano as the primary instrument, with subtle string sections and electronic textures. His vocal style is a clear, emotive tenor with a plaintive quality and restrained delivery. Focus on polished, radio-friendly production with a melancholic, introspective feel.
```

**毛不易 (Mao Buyi)**
```
Mao Buyi-style indie folk pop, blending acoustic simplicity with poetic storytelling. The sound features acoustic guitar, gentle piano, and minimal production. His vocal style is a soft, intimate tenor with a conversational, unpolished quality and emotional sincerity. Focus on stripped-down, lo-fi production with a gentle, contemplative feel.
```

**房东的猫 (Landlord's Cat)**
```
Landlord's Cat-style indie folk, blending warm acoustic textures with gentle female vocals. The sound features acoustic guitar, ukulele, and soft percussion. The vocal style is a sweet, breathy soprano with a warm, intimate quality and delicate phrasing. Focus on cozy, lo-fi production with a gentle, comforting feel.
```

**Abstract styles** (for original mode):

**忧郁民谣 (Melancholic Folk)**
```
Indie folk with melancholic undertones, featuring acoustic guitar, gentle fingerpicking, and sparse arrangements. Vocal style is intimate and vulnerable, with a soft, breathy quality. Focus on lo-fi, atmospheric production with a slow tempo and introspective mood.
```

**城市流行 (Urban Pop)**
```
Modern synth-pop with urban influences, featuring electronic beats, synthesizers, and polished production. Vocal style is smooth and contemporary, with clear articulation and mid-tempo delivery. Focus on sleek, radio-friendly production with a modern, cosmopolitan feel.
```

**复古港乐 (Retro Cantopop)**
```
80s-inspired Cantopop with nostalgic synth textures, featuring vintage synthesizers, drum machines, and warm analog production. Vocal style is smooth and romantic, with a mid-range delivery and subtle vibrato. Focus on retro, warm production with a nostalgic, mid-tempo groove.
```

**摇滚 (Chinese Rock)**
```
Chinese rock with driving energy, featuring electric guitars, powerful drums, and raw vocal delivery. Vocal style is forceful and passionate, with a gritty, unpolished quality. Focus on energetic, guitar-driven production with a fast tempo and rebellious attitude.
```

**古风 (Ancient Chinese Style)**
```
Traditional Chinese-inspired music, featuring guzheng, dizi flute, erhu, and orchestral arrangements. Vocal style is ethereal and operatic, with clear articulation and dramatic phrasing. Focus on cinematic, atmospheric production with a slow, majestic tempo.
```

#### Interactive confirmation

Don't output the Global Style Prompt silently. Present it to the user for confirmation:

> 🎵 **Suno V5 全局风格提示词：**
> 
> `[paste the generated paragraph here]`
> 
> 需要调整吗？比如改变乐器、速度、或氛围。确认后我生成完整的分段指令版本。

Only skip confirmation when the user said "直接导出" or similar.

---

### Part 2: Sectional Prompts + Lyrics (分段指令 + 歌词)

Each lyric section needs TWO bracketed elements:

1. **Section label** — `[Verse 1]`, `[Chorus]`, `[Bridge]`, etc.
2. **Sectional prompt** — `[arrangement/emotion instructions for this section]`

Format:
```
[Verse 1][soft piano intro, intimate vocal delivery, sparse instrumentation]
歌词第一行
歌词第二行
...

[Chorus][full band enters, powerful layered vocals, building emotional intensity]
歌词...
```

#### How to generate Sectional Prompts

**Step 1**: Load the emotion curve from the lyrics output (e.g. `[V1] ▂▃▃▂ intensity 3-4`, `[C] ▇▇█▇ intensity 8`)

**Step 2**: Map emotion intensity to arrangement density:

| Intensity | Arrangement density | Example descriptors |
|---|---|---|
| 1-3 (low) | Sparse, minimal | `soft piano intro`, `intimate vocal`, `sparse instrumentation`, `acoustic guitar only` |
| 4-6 (mid) | Building, layered | `drums enter`, `strings swell`, `building intensity`, `layered vocals` |
| 7-9 (high) | Full, explosive | `full band`, `powerful layered vocals`, `driving beat`, `soaring strings`, `explosive chorus` |
| 10 (peak) | Maximum impact | `climactic peak`, `all instruments`, `vocal belting`, `maximum emotional intensity` |

**Step 3**: Apply song progression logic (sections should evolve, not stay static):

| Section | Typical progression | Example prompts |
|---|---|---|
| **[Intro]** | Sparse, atmospheric | `soft piano intro`, `ambient pad`, `gentle guitar`, `atmospheric opening` |
| **[Verse 1]** | Intimate, building | `intimate vocal delivery`, `sparse instrumentation`, `acoustic foundation`, `gentle rhythm` |
| **[Pre-Chorus]** | Tension building | `building tension`, `drums enter`, `rising intensity`, `anticipation builds` |
| **[Chorus]** | Full, explosive | `full band enters`, `powerful layered vocals`, `driving beat`, `soaring melody`, `emotional peak` |
| **[Instrumental]** | Breathing space | `instrumental break`, `melodic interlude`, `guitar solo`, `piano bridge` |
| **[Verse 2]** | Evolved from V1 | `fuller arrangement than V1`, `drums continue`, `added layers`, `deeper emotion` |
| **[Bridge]** | Contrast/breakdown | `instrumentation strips back`, `intimate moment`, `emotional shift`, `dynamic contrast` OR `building to climax`, `tension rising` |
| **[Final Chorus]** | Peak intensity | `climactic peak`, `all instruments`, `vocal ad-libs`, `maximum emotional intensity`, `extended outro` |
| **[Outro]** | Resolution/fade | `gentle fade`, `piano outro`, `vocals fade`, `atmospheric close`, `reflective ending` |

**Step 4**: Combine intensity + progression + Global Style elements:

Example (陈奕迅 ballad, Verse 1 intensity 3):
```
[Verse 1][soft piano intro, intimate tenor vocal, sparse string accompaniment, gentle and introspective]
```

Example (周杰伦 Chinese style, Chorus intensity 8):
```
[Chorus][full orchestration with guzheng and erhu, driving hip-hop beat, powerful vocal delivery, cinematic and explosive]
```

**Step 5**: Ensure variety — don't repeat the same prompt for multiple sections. Each section should have a distinct instruction reflecting its role in the song's arc.

#### Sectional Prompt Guidelines

**Do:**
- Use concrete musical terms (instruments, techniques, dynamics)
- Reflect the emotion curve intensity
- Show progression across sections (V1 → C → V2 → C should evolve)
- Keep prompts concise (5-10 words, max 15)
- Match the Global Style (if piano-driven ballad, don't suddenly add "heavy guitar")

**Don't:**
- Repeat the same prompt for different sections
- Use vague terms like "emotional" without specifics
- Contradict the Global Style Prompt
- Over-specify (Suno needs room to interpret)
- Include lyrics content in the prompt (that's what the lyrics are for)

---

### Suno V5 Parameters

When presenting the export version, also recommend parameter settings:

**Vocal Gender**
- Male / Female / Duet
- Determined by the target artist or user request

**Weirdness** (0-100, default 50)
- **0-30**: Mainstream, radio-friendly, predictable structure
- **30-60**: Balanced, some creative variation (recommended for most cases)
- **60-80**: Experimental, unusual arrangements, creative risks
- **80-100**: Highly unconventional, avant-garde, unpredictable

Recommendation logic:
- 古风/复古港乐/mainstream pop → 20-40
- 民谣/indie folk/ballad → 40-60
- 摇滚/alternative/experimental → 60-80

**Style Influence** (0-100, default 50)
- **0-30**: AI has high freedom, may deviate from style prompt
- **30-70**: Balanced adherence to style prompt (recommended)
- **70-100**: Strict adherence, less creative freedom

Recommendation: 50-70 for most cases (trust the Global Style Prompt but allow some AI interpretation)

**Audio Influence** (Remix mode only, 0-100)
- Only relevant when user uploads an audio file
- **0-30**: Loose reference, mostly new interpretation
- **30-60**: Balanced (keep melody, reinterpret arrangement)
- **60-100**: Close to original (mainly re-recording/mixing)

For Remix mode (when user wants to keep original melody): recommend 40-60

---

### `[Instrumental]` Insertion Rules

Use the emotion curve to decide where to insert instrumental breaks. Don't insert based on gut feeling.

**Insert `[Instrumental]` when:**
- Between two sections where emotion intensity drops ≥2 points (cool-down moment)
- After the first Chorus and before Verse 2 (standard breathing space)
- Before Bridge (transition into the "third act")

**Don't insert when:**
- Emotion is building continuously (e.g. V1→Pre-Chorus→Chorus with no dip)
- Between Bridge and Final Chorus (momentum should carry through)
- The song is already short (≤8 lines total) — instrumental breaks dilute impact

**Example** (using emotion curve):
```
[V1] ▂▃▃▂ (intensity 3-4)
[C]  ▇▇█▇ (intensity 8)
[Instrumental]          ← drop from 8 to ~4, natural cool-down
[V2] ▃▄▄▃ (intensity 4-5)
[C]  ▇▇█▇ (intensity 8)
[Bridge] ▅▄▃▄ (intensity 5→3→5)
[C]  ▇██▇ (intensity 9, peak)  ← NO instrumental before this
```

---

### Lyrics Length Guidance

Suno has practical limits:
- **Optimal**: 1200-2000 characters (Chinese) per generation
- **Maximum**: ~3000 characters before quality degrades

If lyrics exceed 2000 characters, add a note:
```
【提示】歌词较长（{n}字），建议在 Suno 中分段生成：
- 第一段：生成到 [Chorus] 结束（约 {n1} 字）
- 第二段：使用 Extend 功能续写剩余部分
```

Suggest a natural split point (usually after the first Chorus or before Bridge).

---

### Export Output Format

```
---
【Suno V5 导出版】

**全局风格提示词 (Global Style Prompt):**
[paste the detailed English paragraph here, ending with language tag]

**推荐参数 (Recommended Parameters):**
- Vocal Gender: Male/Female
- Weirdness: 40-60
- Style Influence: 50-70

---

[Intro][atmospheric opening, soft piano]

[Verse 1][intimate vocal delivery, sparse instrumentation, gentle rhythm]
歌词第一行
歌词第二行
...

[Pre-Chorus][building tension, drums enter, rising intensity]
歌词...

[Chorus][full band enters, powerful layered vocals, driving beat, emotional peak]
歌词...

[Instrumental]

[Verse 2][fuller arrangement than V1, drums continue, added layers]
歌词...

[Chorus][full band, powerful layered vocals, soaring melody]
歌词...

[Bridge][instrumentation strips back, intimate moment, emotional shift]
歌词...

[Chorus][climactic peak, all instruments, vocal ad-libs, maximum intensity]
歌词...

[Outro][gentle fade, piano outro, reflective ending]

---
【使用说明】
1. 将"全局风格提示词"粘贴到 Suno 的 Style 输入框
2. 将分段指令+歌词部分粘贴到 Lyrics 输入框
3. 设置推荐参数
4. 如歌词较长，按提示分段生成
```

---

### Export Workflow (Step by Step)

When Suno export is triggered, follow these steps:

**Step 1: Count characters**
- If total lyrics > 2000 chars, add length warning and suggest split point

**Step 2: Generate Global Style Prompt**
- Load artist profile from `style-extraction.md` (7 dimensions)
- Map to Suno's 4 musical elements
- Write 3-5 sentence English paragraph
- Append `, Mandarin` or `, Cantonese` based on actual lyrics language

**Step 3: Present Global Style Prompt for confirmation**
- Show the generated paragraph to user
- Ask if adjustments needed
- Skip confirmation only if user said "直接导出"

**Step 4: Wait for user OK or adjustment**
- If user requests changes, regenerate and re-confirm
- If user approves, proceed to Step 5

**Step 5: Generate Sectional Prompts**
- Load emotion curve from lyrics output
- Map intensity to arrangement density (1-3 sparse, 4-6 building, 7-9 full, 10 peak)
- Apply song progression logic (Intro sparse → Verse intimate → Chorus explosive → Bridge contrast → Outro resolution)
- Ensure variety across sections (don't repeat prompts)

**Step 6: Insert `[Instrumental]` breaks**
- Use emotion curve to decide placement
- Insert when intensity drops ≥2 points
- Standard positions: after first Chorus, before Bridge
- Don't insert when building continuously or before Final Chorus

**Step 7: Generate parameter recommendations**
- Vocal Gender (from artist/request)
- Weirdness (based on genre: mainstream 20-40, indie 40-60, experimental 60-80)
- Style Influence (50-70 for most cases)

**Step 8: Output export version**
- Format: Global Style Prompt + Parameters + Sectional Prompts + Lyrics
- Strip metadata header (【模式】【语言】etc.)
- Add usage instructions
- If split needed, mark split point with `--- 建议在此处分段生成 ---`

---

## Udio Format

Udio uses a similar tag system with minor differences:

**Differences from Suno:**
- Udio prefers shorter sections (4 lines max per tag)
- Udio supports `[Verse]` without numbers
- Style is set separately in the UI, not in the lyrics text
- Sectional prompts work the same way

**When exporting for Udio:**
- Still output the Global Style Prompt as a reference for the user to paste into Udio's style field
- Keep sectional prompts in the lyrics
- Suggest breaking longer sections into 4-line chunks

---

## Remix Mode (Audio Upload)

When the user wants to upload an audio file and remix it (keep original melody, change arrangement/vocals):

**Additional parameter:**
- **Audio Influence**: 40-60 (balanced — keep melody, reinterpret arrangement)

**Workflow adjustments:**
- Global Style Prompt should describe the TARGET style (not the original)
- Sectional Prompts should guide the reinterpretation
- Add a note: `【Remix 模式】上传原始音频后，设置 Audio Influence 为 40-60，保持旋律但重新演绎编曲和人声`

**Copyright warning:**
- Suno has copyright detection for commercial releases
- Suggest using user's own recording or cover versions
- Lower Audio Influence (20-40) reduces copyright risk

---

## Rules

- Strip the metadata header (【模式】【语言】etc.) from the export version — Suno doesn't need it
- Keep section tags and sectional prompts
- Place `[Instrumental]` based on emotion curve cool-down points (see rules above)
- Don't add `[Intro]` or `[Outro]` unless the song structure calls for it
- The export is an ADDITIONAL output — always show the standard format first, then the export version below it
- Language in Global Style Prompt must match the actual lyrics language, not the artist's "default" language
- Global Style Prompt must be a detailed paragraph (3-5 sentences), NOT a tag list
- Sectional Prompts must vary across sections — don't repeat the same instruction
- Always present Global Style Prompt for user confirmation before generating the full export (unless user said "直接导出")
