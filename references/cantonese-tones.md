# Cantonese Tones and Lyric Harmony (协音)

Loaded only when the Cantonese tone-harmony mode is on. Cantonese has 9 tonal categories; lyrics must align tone height with melody height or the listener hears a different word.

## The 9-tone system

| # | Name | Pitch | Example | Jyutping |
|---|---|---|---|---|
| 1 | 阴平 (high level / high falling) | high | 詩 | si1 |
| 2 | 阴上 (high rising) | high-mid → high | 史 | si2 |
| 3 | 阴去 (mid level) | mid | 試 | si3 |
| 4 | 阳平 (low falling) | low | 時 | si4 |
| 5 | 阳上 (low rising) | low → mid | 市 | si5 |
| 6 | 阳去 (low level) | low-mid | 是 | si6 |
| 7 | 阴入 (high checked) | high, short stop | 識 | sik1 |
| 8 | 中入 (mid checked) | mid, short stop | 錫 | sek3 |
| 9 | 阳入 (low checked) | low, short stop | 食 | sik6 |

The three checked tones (7/8/9) end in `-p`, `-t`, or `-k` and are short.

## Harmony principle

Group tones into three pitch buckets:

- **High bucket**: 1, 2, 7
- **Mid bucket**: 3, 8
- **Low bucket**: 4, 5, 6, 9

A character whose tone bucket clashes with the melody pitch produces "拗音" — the listener hears a different word than the one written.

Working principle when writing:

- Melody high note → choose a character in the High bucket.
- Melody low note → choose a character in the Low bucket.
- Melody mid note → Mid bucket (or High/Low one step away if Mid is hard to find).
- Within the same bucket, tone subtleties matter less, but rising tones (2, 5) feel best on rising melody.

## Annotation workflow

When the user asks for tone-harmony output:

1. Convert each character to its tone number using the table above.
2. For multi-tonal characters (多音字), pick the tone for the in-context reading. If unsure, search "字 + 粤语读音" or note both options.
3. Append the tone numbers as a space-separated list in brackets after each line.

Example:

```
我願意               [5 6 1]
為你寫一首歌          [4 5 2 1 7 1]
```

## Common pitfalls

- **Rhyme lock**: when the rhyme word is fixed, the tone of the rhyme position is locked too. If it clashes with the melody, swap to a different rhyme family. Don't bend the meaning to keep the rhyme.
- **Sentence-final particles** (啊 / 嗎 / 呢 / 喎): they take their own tone and often land on melody resolution points. Choose the particle whose tone matches.
- **Names and proper nouns**: their tones can't be swapped. If the melody clashes, restructure the line so the name lands on a different beat.
- **Checked tones (7/8/9)**: they're short. Don't put them on long held notes — the listener will hear a held vowel that feels wrong.

## Notation in output

Keep the tone numbers compact: digits separated by spaces in square brackets, right after the line. Don't mix in pitch annotations or rhyme marks unless the user asked.
