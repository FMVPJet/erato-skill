# Changelog

All notable changes to the Erato skill will be documented in this file.

## [1.2.1] - 2026-05-20

### Changed (Suno Export Interaction)
- **Style prompt 交互确认**: 导出时先展示 style tag 让用户确认/调整，而非静默输出
- **Language tag 动态化**: style tag 的语言标签根据实际歌词语言决定，不再绑定歌手默认语言
- **`[Instrumental]` 基于情绪曲线**: 用情绪强度下降点决定间奏位置，替代模糊的"feels natural"
- **歌词长度提示**: 超过 2000 字时提醒用户分段生成，并建议分割点
- **被动触发提示**: 未主动提及 Suno 时，iteration prompt 末尾附带导出提示
- **导出工作流程化**: 新增 6 步 export workflow（计数→生成→确认→等待→输出→分段标记）
- **Artist 映射表精简**: 移除表中硬编码的语言前缀，语言由输出决定

## [1.2.0] - 2026-05-20

### Added (Music & Craft Enhancements)
- **情绪动力学** (`emotion-dynamics.md`): 5 种经典情绪曲线模板（渐强/波浪/倒叙/平台/爆发-回落），写作前设计情绪强度，确保 V1-C 强度差 ≥3
- **意象库** (`imagery-library.md`): 按情感分类的意象库（孤独/思念/失恋/爱情/成长/希望），意象搭配原则（具体+抽象/大+小/通感/意外组合），按歌手风格选择意象
- **歌词节奏感** (`lyric-rhythm.md`): 音节分组模式、节奏与情绪的关系、副歌 hook 节奏设计、呼吸点规则
- **声线特征** (style-extraction.md 第七维度): 音域/速度/转音/气口/咬字，影响歌词节奏和用字选择
- **韵母情感色彩** (rhyme-guide.md): 开阔韵（ang/a/ao）适合副歌爆发，收敛韵（i/u/en）适合内心戏，含粤语韵母色彩
- **画面感自检** (anti-ai-patterns.md): 4 层画面感层次（静态→动态→感官→情感投射），Visual Test 检查法

### Changed
- **Imitation 模式**: 新增"设计情绪曲线"步骤，写作时加载 imagery-library 和 lyric-rhythm
- **Creation 模式**: 新增"设计情绪曲线"步骤，写作时考虑韵母情感色彩、节奏设计、意象选择
- **Original 模式**: 新增"设计情绪曲线"步骤，写作时加载全部新参考文件
- **Style extraction**: 从六维度升级为七维度（歌手增加声线特征）
- **Resources 列表**: 更新以反映新增的 3 个参考文件和已增强的 3 个文件

## [1.1.0] - 2026-05-20

### Fixed
- **粤语协音模式逻辑矛盾**: 区分"有旋律"和"无旋律"两种场景。有旋律时主动优化声调匹配，无旋律时仅标注不优化
- **Melody-First 触发条件不清晰**: 明确要求用户提供具体约束（字数/节奏/音高），"帮我填词"不再自动触发
- **WebSearch 执行时机模糊**: 改为"trigger WebSearch"而非"load strategy"，确保真正执行搜索
- **Self-check 重复检查**: 移除 mode-prompts.md 中与 Singability check 重复的检查项

### Added
- **Copyright check 质量门**: 在 imitation 和 creation 模式中检查是否复制了原曲或艺人的标志性歌词
- **Final Chorus 变化规则**: 补充到 Original 模式（之前只在 Imitation 和 Creation 中提到）
- **Quick Start 指南**: 在 SKILL.md 开头添加快速上手指南
- **5 个新测试用例**: Test Case 11-15，覆盖 Melody-First 组合、Multi-Variant、Hook Candidates、Suno Export、改结构
- **版本号追踪**: 在 SKILL.md frontmatter 中添加 version 字段

### Changed
- **Artist profile 保存条件明确化**: 只保存通过 WebSearch 获得的主流艺人档案，不保存已知艺人或小众独立音乐人
- **WebSearch 自检标准提升**: 要求能说出 3+ 代表作且能用具体例子描述 4+ 维度，否则触发搜索

### Documentation
- 创建 CHANGELOG.md 记录版本变更
- 更新 CLAUDE.md 以反映新的架构和工作流程

## [1.0.0] - 2026-05-19

### Added
- 初始版本发布
- 三种创作模式：仿写、创作、原创
- 粤语协音标注功能
- Melody-First 填词模式
- Suno/Udio 导出功能
- 10 个基础测试用例
