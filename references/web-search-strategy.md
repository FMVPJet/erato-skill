# Web Search Strategy for Lyrics

When you need to find song lyrics or artist information via WebSearch, follow this escalation strategy. Don't give up after one failed search.

## Known tool limitations

- **WebSearch** returns titles and snippets only, not full page content
- **WebFetch** may be blocked by security policies on lyrics/music sites
- Chinese lyrics databases (QQ音乐, 网易云) use dynamic loading and anti-scraping
- **Practical reality**: in most environments, you cannot reliably retrieve full lyrics via tools alone

Given these limitations, the strategy prioritizes **fast fallback to asking the user** over exhaustive searching. The goal of searching is to:
1. Confirm the song exists (so you don't ask the user for lyrics to a non-existent song)
2. Pick up any structural/thematic clues from titles and snippets
3. Find the artist's lyricist (for style profiling)

## Lyrics search escalation

### Attempt 1: Direct query
```
{歌手} {歌名} 歌词
```

### Attempt 2: Add platform keywords
```
{歌名} {歌手} 歌词 site:music.163.com OR site:y.qq.com
```
or
```
{歌名} {歌手} 完整歌词 网易云
```

### Attempt 3: Broader search
```
{歌手} {歌名} lyrics
```
or
```
"{歌名}" "{歌手}"
```
(Exact match with quotes)

### Attempt 4: Verify the song exists
```
{歌手} 专辑 {歌名}
```
or
```
{歌手} 歌曲列表 discography
```

If attempt 4 also fails to confirm the song exists, the song may:
- Not exist (user misremembered the title)
- Be extremely new (not yet indexed)
- Be an unreleased/live-only track

## When to stop searching and ask the user

Stop after **2 failed attempts** (not 4) if:
- The song title seems unusual or possibly misspelled
- The artist is well-known but the song doesn't appear in any results
- Results return completely unrelated content

In these cases, ask the user:
> 我搜索了「{歌名}」但没有找到相关歌词。可能是：
> 1. 歌名有误？
> 2. 非常新的歌还没被收录？
> 3. 你能直接贴歌词给我？

## Artist style search

When profiling an unfamiliar artist:

### Attempt 1:
```
{歌手} 歌词风格 代表作
```

### Attempt 2:
```
{歌手} 作词人 御用
```
(Find who writes for them)

### Attempt 3:
```
{歌手} 音乐风格 评价
```

## Rules

- **Max 3 search attempts** for lyrics before asking the user
- **Max 2 search attempts** for artist profiling before asking the user
- **Never fabricate lyrics** based on partial search results
- **Never assume** a song's content from its title alone
- If the song title contains the artist's name or a pun (like "纯妹妹" for 单依**纯**), note this — it may be a fan nickname, unreleased track, or self-referential song that's harder to find
