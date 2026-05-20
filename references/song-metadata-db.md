# Song Metadata Database (参考歌曲元数据)

A lightweight database of well-known songs' structural metadata. Used to quickly look up structure, line counts, and emotional arcs without needing full lyrics (which have copyright concerns).

## Format

Each entry contains only structural/analytical metadata — never full lyrics or memorable lines.

## Mandarin Pop

### 周杰伦 / 方文山

- **《青花瓷》**: V-V-C-V-C-C | 7字/行为主 | 韵：ang/eng | 情绪：平静→深情→释然 | 古典意象密集
- **《晴天》**: V-PC-C-V-PC-C-B-C | 8-10字/行 | 韵：an/ian | 情绪：怀念→甜蜜→遗憾 | 校园/天气意象
- **《稻香》**: V-C-V-C-B-C | 7-9字/行 | 韵：ang | 情绪：低落→振作→温暖 | 乡村/童年意象
- **《听妈妈的话》**: V-C-V-C | 口语化长句 | 韵：松散 | 情绪：叙事→感恩 | 日常生活

### 陈奕迅 / 林夕

- **《富士山下》(粤)**: V-C-V-C-B-C | 9-12字/行 | 韵：eui/ui | 情绪：纠结→释然→祝福 | 自然/距离意象
- **《十年》**: V-C-V-C | 7-10字/行 | 韵：an/ian | 情绪：平静→感伤→接受 | 时间/日常意象
- **《K歌之王》**: V-C-V-C-B-C | 长短交替 | 韵：ang | 情绪：自嘲→爆发→无奈 | 音乐/表演意象
- **《浮夸》(粤)**: V-PC-C-V-PC-C | 短句急促 | 韵：aa | 情绪：压抑→爆发→疯狂 | 表演/面具意象

### 林俊杰

- **《江南》**: V-C-V-C-B-C | 7-9字/行 | 韵：an | 情绪：忧伤→深情→释然 | 雨/水/江南意象
- **《修炼爱情》**: V-PC-C-V-PC-C-B-C | 8-10字/行 | 韵：ing | 情绪：回忆→痛→成长 | 时间/修行意象
- **《她说》**: V-C-V-C | 8-10字/行 | 韵：e/uo | 情绪：叙述→心疼→无奈 | 对话/距离意象

### 王菲 / 林夕

- **《红豆》**: V-C-V-C | 7-9字/行 | 韵：ou/u | 情绪：平静→思念→释然 | 日常/等待意象
- **《匆匆那年》**: V-C-V-C-B-C | 8-10字/行 | 韵：an | 情绪：怀念→遗憾→放下 | 时间/青春意象

### 李宗盛

- **《山丘》**: Through-composed | 长句叙事 | 韵：松散 | 情绪：平静→感慨→释然 | 山/路/中年意象
- **《给自己的歌》**: V-C-V-C | 口语化 | 韵：松散 | 情绪：自省→坦然 | 日常/独处意象
- **《当爱已成往事》**: V-C-V-C-B-C | 7-9字/行 | 韵：i | 情绪：痛→回忆→接受 | 往事/距离意象

## Cantonese Pop

### 黄伟文

- **《喜帖街》(谢安琪)**: V-C-V-C-B-C | 9-11字/行 | 韵：aai/ai | 情绪：怀旧→感伤→释然 | 街道/城市变迁
- **《陀飞轮》(陈奕迅)**: V-C-V-C | 8-10字/行 | 韵：an | 情绪：追逐→反思→顿悟 | 时间/手表/都市

### 林夕 (粤语)

- **《富士山下》(陈奕迅)**: 见上
- **《再见二丁目》(杨千嬅)**: V-C-V-C | 9-11字/行 | 韵：ing | 情绪：孤独→思念→自愈 | 日本/城市/独行

## How to use

- **Imitation mode**: look up the source song's structure, line counts, and emotional arc. Use these as hard constraints.
- **Creation mode**: find songs with similar themes by the target artist. Use their typical structure and line counts as a starting template.
- **Original mode**: find songs with similar emotional arcs. Use their structure as inspiration (not constraint).

## Adding entries

When you profile a new song during imitation mode, add its metadata here for future reference. Format:
```
- **《歌名》**: structure | line-length | rhyme | emotional-arc | imagery-type
```
