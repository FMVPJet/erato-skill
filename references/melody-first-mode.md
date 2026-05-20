# Melody-First Mode (填词模式)

When the user provides rhythmic or melodic constraints (syllable counts, stress patterns, or pitch contours), switch to melody-first writing. This is how professional 填词人 work — the melody exists first, and words are fitted to it.

## Trigger

Activate this mode when the user provides ANY of:
- Per-line syllable/character counts (e.g. "7-7-5-7")
- Stress/accent patterns (e.g. "x-X-x-X-x-X-x" where X = stressed)
- Pitch contours (e.g. "高-中-低-高-高")
- A reference melody (hummed, described, or named)
- Explicit request: "帮我填词" / "按这个旋律写词"

## Constraints hierarchy

When filling words to melody, priorities (highest first):

1. **Character count** — must match exactly (±0, not ±2 like imitation mode)
2. **Tone-melody match** (Cantonese) — tone bucket must align with pitch
3. **Stress alignment** — stressed syllables on strong beats
4. **Meaning** — the words must make sense and serve the theme
5. **Rhyme** — maintain the rhyme scheme
6. **Singability** — open vowels on high/held notes

## Input formats the user might provide

### Format A: Character counts only
```
第一段：7-7-5-7-7-5
副歌：8-8-6-8
```

### Format B: Counts + stress
```
7: x-X-x-X-x-X-x
5: X-x-X-x-X
```
(X = stressed/strong beat, x = weak beat)

### Format C: Pitch contour (Cantonese)
```
高-高-中-低-中-高-高
中-低-低-中-高-高
```

### Format D: Reference song
```
按《富士山下》的旋律填新词
```
(Extract character counts and structure from the reference)

## Workflow

1. Parse the constraints into a per-line spec:
   - Exact character count
   - Stress positions (if given)
   - Pitch contour (if given)
   
2. For each line, generate candidates that satisfy:
   - Exact character count (hard constraint)
   - Tone/pitch match (hard constraint for Cantonese with contour)
   - Stress alignment (soft constraint — prefer but don't force)
   
3. Assemble lines into sections, checking:
   - Rhyme scheme holds
   - Meaning flows between lines
   - No awkward enjambment at phrase boundaries

4. Run singability check on the result

## Output format addition

When in melody-first mode, add to the header:

```
【填词模式】是
【字数格律】7-7-5-7 / 8-8-6-8 / ...
```

## Combining with other modes

Melody-first can combine with any of the three main modes:
- 仿写 + 填词: use the original song's melody constraints
- 创作 + 填词: user gives melody, skill writes in target artist's style
- 原创 + 填词: user gives melody, skill writes in specified abstract style

## Limitations

- Without actual audio, "pitch contour" is approximate
- Stress patterns in Chinese are less rigid than in English — treat as preference, not law
- If constraints conflict with meaning, meaning wins (flag the conflict to the user)
