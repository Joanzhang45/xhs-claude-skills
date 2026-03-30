# RedNote to Obsidian — Claude Code Skills

> Turn [RedNote (小红书)](https://www.xiaohongshu.com) posts into concise, actionable Obsidian notes — with video transcription.

<p align="center">
  <code>/xhs</code> &nbsp;·&nbsp; <code>/xhs-batch</code> &nbsp;·&nbsp; <code>/xhs-analyze</code>
</p>

---

## What it does

| Command | Description |
|---------|-------------|
| `/xhs <url>` | Extract a single post — text, images, video transcription |
| `/xhs-batch <urls>` | Batch extract multiple posts |
| `/xhs-analyze [keyword]` | Analyze saved posts — summarize, compare, find patterns |

## How it works

```
RedNote URL → Chrome cookies auth → parse __INITIAL_STATE__
                                          │
                          ┌───────────────┼───────────────┐
                          ▼               ▼               ▼
                       Text            Images          Video
                     (desc)          (urlDefault)    (stream URL)
                          │               │               │
                          │               │          curl → ffmpeg
                          │               │            → mlx-whisper
                          │               │               │
                          └───────────────┼───────────────┘
                                          ▼
                                  Obsidian Markdown
                              (scan in 5s, decide to dig)
```

- No MCP server, no headless browser, no Playwright
- Just cookies + HTTP + local whisper — fully offline after auth

## Output

```
xhs/
├── 2026-03-22 社区反馈训练AI判断力.md
├── 2026-03-28 AI学习法三问框架.md
├── 2026-03-29 世界模型反坍塌证明.md
├── img/
└── video/
```

Each note is a **decision tool**, not a knowledge dump:

```markdown
# One-line insight (not a description)

Core argument in 2-3 sentences. Direct. Opinionated.

**Relevance:** Why this matters to you — one line.

**Worth digging?** Yes/No + one-line reason.

> [!tip]- Details                    ← collapsed by default
> Full structured content...

> [!info]- Metadata                  ← collapsed by default
> Source, date, stats, tags...
```

## Prerequisites

- **macOS** with Apple Silicon (for mlx-whisper; Intel works but slower)
- **`ffmpeg`** — `brew install ffmpeg`
- **`mlx-whisper`** — `pip install mlx-whisper` (video transcription only)
- **Chrome cookies** — export once, refresh when expired

### Cookie export

Open Chrome DevTools on xiaohongshu.com → Console:

```javascript
copy(JSON.stringify(document.cookie.split('; ').map(c => {
  const [name, ...rest] = c.split('=');
  return { name, value: rest.join('='), domain: '.xiaohongshu.com', path: '/',
    expires: Date.now()/1000 + 86400*30, size: name.length + rest.join('=').length,
    httpOnly: false, secure: false, session: false, priority: 'Medium',
    sameParty: false, sourceScheme: 'Secure', sourcePort: 443 };
})))
```

Save clipboard → `~/cookies.json`.

## Install

```bash
cp commands/xhs*.md ~/.claude/commands/
```

Then edit paths in each skill file if your setup differs:
- `~/cookies.json` — cookies location
- `~/Documents/Obsidian Vault/xhs` — output directory

## Customization

The **"Relevance"** section is personalized to the user's background. Edit the user description in `commands/xhs.md` (step 4) to match your own context — researcher, developer, designer, whatever you are.

## License

MIT

---

# RedNote 转 Obsidian — Claude Code 技能包

> 把[小红书](https://www.xiaohongshu.com)帖子变成简洁、可操作的 Obsidian 笔记 — 支持视频语音转录。

---

## 功能

| 命令 | 说明 |
|------|------|
| `/xhs <链接>` | 提取单个帖子 — 文字、图片、视频转录 |
| `/xhs-batch <链接列表>` | 批量提取多个帖子 |
| `/xhs-analyze [关键词]` | 分析已保存的帖子 — 总结、对比、发现模式 |

## 工作原理

1. 用 Chrome 导出的 cookies 请求帖子页面
2. 从 `__INITIAL_STATE__` 解析全部数据（无需 MCP、无头浏览器）
3. 视频帖子：下载 → ffmpeg 提取音频 → mlx-whisper 本地转录
4. 输出精简笔记：扫一眼决定要不要深挖

## 输出格式

```
xhs/
├── 2026-03-22 社区反馈训练AI判断力.md
├── 2026-03-28 AI学习法三问框架.md
├── 2026-03-29 世界模型反坍塌证明.md
├── img/
└── video/
```

每条笔记是**决策工具**，不是信息搬运：

- **标题**：一句话洞察（不是描述）
- **2-3句话**：核心论点，直接给判断
- **与我的关联**：一句话，为什么跟我有关
- **值得深挖吗**：是/否 + 理由
- **折叠详情**：完整内容，点开才看到

## 依赖

- **macOS**（Apple Silicon 推荐，转录更快）
- **ffmpeg**：`brew install ffmpeg`
- **mlx-whisper**：`pip install mlx-whisper`（仅视频转录需要）
- **Chrome cookies**：导出一次，过期刷新

### 导出 Cookies

Chrome 打开小红书 → DevTools Console 运行：

```javascript
copy(JSON.stringify(document.cookie.split('; ').map(c => {
  const [name, ...rest] = c.split('=');
  return { name, value: rest.join('='), domain: '.xiaohongshu.com', path: '/',
    expires: Date.now()/1000 + 86400*30, size: name.length + rest.join('=').length,
    httpOnly: false, secure: false, session: false, priority: 'Medium',
    sameParty: false, sourceScheme: 'Secure', sourcePort: 443 };
})))
```

粘贴保存到 `~/cookies.json`。

## 安装

```bash
cp commands/xhs*.md ~/.claude/commands/
```

如果路径不同，修改每个 skill 文件里的：
- `~/cookies.json` — cookies 位置
- `~/Documents/Obsidian Vault/xhs` — 输出目录

## 个性化

笔记中的"与我的关联"部分会根据用户背景生成。编辑 `commands/xhs.md` 步骤 4 中的用户描述，改成你自己的身份和工作方向即可。

## 许可

MIT
