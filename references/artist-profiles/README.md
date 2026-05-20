# Artist Profile Persistence

When you profile an artist using the six-dimension method, save the result here so future invocations can skip the profiling step.

## File naming

One file per artist: `artist-profiles/{artist-name}.md`

Use the artist's most common name (e.g. `陈奕迅.md`, `林俊杰.md`, `周杰伦.md`).

## File format

```markdown
# {Artist Name} — Lyrical Profile

**Last updated**: YYYY-MM-DD
**Primary lyricist(s)**: {who writes for them}
**Representative songs**: {3-5 titles}

## Six Dimensions

### 1. Vocabulary preference (用词偏好)
{specifics}

### 2. Viewpoint (视角)
{specifics}

### 3. Rhetorical density (修辞密度)
{specifics}

### 4. Signature imagery (典型意象)
{specifics}

### 5. Sentence patterns (句式特征)
{specifics}

### 6. Emotional register (情感基调)
{specifics}

## Notes
{any additional observations, era differences, etc.}
```

## When to save

- After successfully profiling an artist for the first time in a session
- Only save if you're confident in the profile (passed the 3+ songs self-check)
- Don't save profiles derived solely from WebSearch with limited results

## When to load

- Before profiling an artist, check if `artist-profiles/{name}.md` exists
- If it exists, load it instead of re-profiling from scratch
- Still verify the profile feels correct for the specific task — if the user is asking about a specific era of the artist, the saved profile may need adjustment

## When to update

- If you notice the saved profile is incomplete or inaccurate during use
- If the user corrects something about an artist's style
- Add a note about era differences if relevant (e.g. "early 林夕 vs late 林夕")
