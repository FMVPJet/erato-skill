# Tone-Harmony Scoring (协音评分)

## Overview

When Cantonese tone-harmony mode is on, after annotating each line with tone numbers, run this scoring system to identify potential 拗音 (tone-melody clashes) and give the user a quality score.

## Scoring Method

### Step 1: Identify high-risk positions

Not every character position matters equally. These positions are most sensitive to tone clashes:

**High-risk positions** (weight × 3):
- Line-final character (句尾字) — the ear lingers here
- Rhyme position (押韵字) — listener expects resolution
- First character of a line (句首字) — sets the tonal expectation
- Held notes (长音位) — if melody info is available

**Medium-risk positions** (weight × 2):
- Stressed syllables in the phrase
- Characters before a pause/breath

**Low-risk positions** (weight × 1):
- All other characters (passing tones)

### Step 2: Check each character against its bucket

For each character, determine if its tone bucket matches the expected melodic direction:

| Tone bucket | Tones | Expected melody |
|---|---|---|
| High | 1, 2, 7 | High notes, rising phrases |
| Mid | 3, 8 | Middle register |
| Low | 4, 5, 6, 9 | Low notes, falling phrases |

**Without melody input** (most common case): check for internal consistency within a line:
- Characters in the same phrase position across lines should have similar tone heights
- Rhyme words across lines should be in the same or adjacent bucket
- Avoid placing a tone-1 (high) character where the surrounding context implies low register

**With melody input** (if user provides pitch contour): directly compare tone bucket to melody pitch.

### Step 3: Calculate score

```
Score = 100 - (penalty points)

Penalty per clash:
- High-risk position clash: -8 points
- Medium-risk position clash: -4 points  
- Low-risk position clash: -2 points

Bonus:
- Rhyme words all in same tone bucket: +5
- Line-final tones follow a consistent pattern (e.g. all mid/low): +3
```

### Step 4: Grade and report

| Score | Grade | Meaning |
|---|---|---|
| 90-100 | A | Excellent harmony, professional level |
| 75-89 | B | Good, minor issues that most listeners won't notice |
| 60-74 | C | Acceptable, some noticeable clashes |
| Below 60 | D | Needs revision, multiple obvious clashes |

## Output Format

After the lyrics with tone annotations, append a scoring section:

```
---

【协音评分】82/100 (B)

⚠️ 潜在拗音：
- [Verse 1] 第3行「xxx」第4字「字」(tone 6/低) 处于句首高位，建议换为同义高声字
- [Chorus] 第2行 押韵字「xxx」(tone 4/低) 与其他押韵字 (tone 1, 1, 3) 不一致

✅ 优点：
- 副歌押韵字声调统一（全部 mid/high bucket）
- 句尾字走向一致
```

## When to apply

- **Always** when tone-harmony mode is on
- Run scoring **after** writing and annotating, as a final quality gate
- If score < 75 (grade C or below), automatically attempt to fix the flagged positions before presenting to user
- If score ≥ 75, present as-is with the score report

## Fixing flagged positions

When a clash is identified:

1. Find synonyms or near-synonyms in the correct tone bucket
2. If no good synonym exists, restructure the phrase to move the problematic character to a low-risk position
3. If restructuring breaks the meaning, keep the original and note it as an accepted trade-off
4. Re-score after fixes

## Limitations

- Without actual melody input, scoring is based on internal tonal consistency and conventional patterns — it's an approximation, not absolute
- The model's Cantonese tone knowledge may have errors for rare characters — flag uncertainty with "(?)" in annotations
- Checked tones (7/8/9) on long held notes are always flagged regardless of bucket match (they're too short for sustained notes)
