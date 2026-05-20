# Extracting an Artist's Lyrical Style

Use this when imitating, creating for, or mixing the style of a known lyricist or singer.

## Method: Six Dimensions

For any artist, profile their voice along these six axes. Note specifics — vague descriptors like "abstract" or "emotional" are useless for actually writing.

**Note**: For singers (not lyricists), also add dimension 7 (vocal characteristics) to account for their voice's physical properties.

### 1. Vocabulary preference (用词偏好)

- Words they reach for repeatedly (concrete nouns, distinctive verbs).
- Words they avoid (e.g. some refuse cliché 情情爱爱 vocabulary).
- Register: classical / bookish / colloquial / slang.

### 2. Viewpoint (视角)

- Person: first / second / third / omniscient narrator.
- Gender perspective and age implied by the writing.
- Distance: confessional vs. observational vs. allegorical.

### 3. Rhetorical density (修辞密度)

- Direct statement vs. metaphor vs. metonymy vs. synesthesia vs. pun.
- How often per stanza? Heavy metaphor (林夕) vs. cinematic concrete (方文山) vs. plainspoken (李宗盛).

### 4. Signature imagery (典型意象)

- Recurring physical objects, places, weather, time-of-day.
- These are *category* signatures — pull the *kind* of imagery, not the literal phrase.
  - 林夕: time, dust, glass, distance, fate
  - 方文山: classical/wuxia objects, calligraphy, weather, dynasty motifs
  - 黄伟文: urban detail, cosmetics, brand, daily-life specifics
  - 李宗盛: aging, regret, ordinary domestic scenes

### 5. Sentence patterns (句式特征)

- Long-line vs. short-line balance.
- Parallelism / antithesis frequency.
- Line-end habits: cliffhanger, declaration, image hold, question.
- Whether they enjamb across the bar (lyric flows past the musical phrase) or stay inside.

### 6. Emotional register (情感基调)

- Fatalism / self-mockery / detachment / fervor / tenderness / nostalgia / restraint.
- The *default* mood the artist returns to even when the literal subject changes.

### 7. Vocal characteristics (声线特征)

Consider the singer's voice, which affects lyric rhythm and word choice:

- **音域 (Range)**: High register / mid register / low register strengths?
- **速度 (Speed)**: Fast articulation (周杰伦, 蔡依林) vs slow, sustained notes (陈奕迅, 张学友)?
- **转音 (Runs/Melisma)**: Good at vocal runs (张惠妹, 林俊杰) → can design complex emotions on long notes
- **气口 (Breath)**: Long breath capacity (suits long phrases) vs short breath (needs frequent breath points)?
- **咬字 (Articulation)**: Clear enunciation (suits fast songs, rap) vs soft/lazy (suits slow songs, atmosphere)?

**实战应用**:
- **周杰伦** → Short phrases, fast rhythm, many pauses, suits narrative and image stacking
- **陈奕迅** → Long phrases, sustained notes, emotional turns, suits inner monologue
- **邓紫棋** → High note explosions, vocal runs, design emotional climax in high register
- **李宗盛** → Spoken-word style, colloquial, many breath points, like telling a story
- **林俊杰** → Mid-range comfort, smooth transitions, suits delicate emotions and piano-driven songs
- **张惠妹** → Powerful belting, wide range, suits big emotional releases and anthems

## Process

When given an artist:

1. **Check for saved profile first.** Look for `references/artist-profiles/{artist-name}.md`. If it exists, load it and skip to step 6.
2. Recall 3–5 representative songs from model memory.
3. Fill in the six dimensions with concrete details (not adjectives).
4. **Self-check**: Can you name 3+ representative songs by this artist with specific titles? Can you describe at least 4 of the 6 dimensions with concrete examples (not vague adjectives)? If not, you don't know them well enough — proceed to step 5.
5. **Trigger WebSearch** for the artist + "歌词 风格" or for representative works and reviews. Follow the escalation strategy in `web-search-strategy.md` (max 2 attempts for artist profiling).
6. If a dimension still resists profiling after search, ask the user for 2–3 representative lyrics and re-derive.
7. **Optionally save the profile.** If you successfully profiled the artist via WebSearch (not from memory) AND the artist is mainstream/likely to be requested again, save to `references/artist-profiles/{artist-name}.md`. Don't save profiles for one-off indie artists or artists you already knew from memory.

## Quick Reference: Major Mandarin and Cantonese Lyricists

**Note**: These are style snapshots from the early 2020s. Lyricists' voices evolve over time (e.g. 林夕's early work vs. late-career work differs significantly). When working with a specific song, re-profile using the six-dimension method rather than relying solely on this table.

These are starting points only — use the six-dimension method to flesh out, and don't assume any of these is exhaustive.

- **林夕 (Albert Leung)**: heavy metaphor, fatalistic, time/distance/glass imagery, second-person and omniscient mixing, philosophical detachment over raw feeling. Cantonese and Mandarin both.
- **黄伟文 (Wyman Wong)**: urban specifics, brand and cosmetic detail, sharp self-aware humor, twist endings, second-person address common. Cantonese mainly.
- **方文山**: classical-aesthetic vocabulary, cinematic concrete imagery, dynasty/wuxia motifs, syllable-conscious craftsmanship for Jay Chou's melodies.
- **姚谦**: melancholic restraint, female perspective, daily-life small moments, plainer vocabulary than 林夕.
- **李宗盛**: plainspoken, narrative, middle-aged regret and observation, almost spoken-word at times.
- **周耀辉**: intellectual and abstract, poetic logic, philosophical themes, sometimes deliberately strange syntax.
- **小柯**: warm, conversational, simple imagery, emotional directness.
- **林若宁**: 林夕's protégé; structurally tight, similar metaphorical style, often a bit cooler in temperature.

For singers (rather than lyricists), the relevant question is *who writes for them*. Examples:

- **陈奕迅**: mainly 林夕 and 黄伟文 — so combine those style notes.
- **周杰伦**: mostly 方文山, sometimes 周杰伦 himself or 黄俊郎.
- **王菲**: 林夕 (Cantonese era), various (Mandarin era).

## When the artist isn't in the table

Apply the six-dimension method directly from memory or `WebSearch`. If the singer doesn't write their own lyrics, search for "歌手名 + 御用作词 / 主要作词人" first to find the relevant lyricist's voice.

## Avoiding plagiarism

Iconic lines belong to their songs. Stay away from:

- 林夕 / 王菲: "你给我一滴泪，我看见你心中所有的海洋"
- 方文山 / 周杰伦: "天青色等烟雨，而我在等你"
- 林夕 / 陈奕迅: "如果你太累，及时地道别没罪"
- … and any line a listener would identify as "from that song".

Use the *manner* of writing such a line, not the line itself.
