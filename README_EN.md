<div align="center">

# 📕 RedNote to Obsidian

### Turn RedNote posts into actionable Obsidian notes — with video transcription

[![Platform](https://img.shields.io/badge/macOS-black?logo=apple&logoColor=white)](https://github.com/chenxiachan/xhs-claude-skills)
[![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-7C3AED)](https://docs.anthropic.com/en/docs/claude-code)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

**[中文](README.md)** &nbsp;|&nbsp; English

</div>

---

## ✨ What it does

| Command | Description |
|:--------|:------------|
| 📄 `/xhs <url>` | Extract a single post — text, images, video transcription |
| 📦 `/xhs-batch <urls>` | Batch extract multiple posts |
| 🔍 `/xhs-analyze [keyword]` | Analyze saved posts — summarize, compare, find patterns |

## 🏗 How it works

```
 📕 RedNote URL
     │
     ▼
 ┌─────────────────────────┐
 │  🍪 Cookie auth          │  ← First-run: auto-guided 30s setup
 └────────────┬────────────┘
              ▼
 ┌─────────────────────────┐
 │  📦 Parse page data      │  ← One HTTP request, no browser needed
 └────┬──────┬──────┬──────┘
      ▼      ▼      ▼
    📝       🖼     🎬
   Text    Images  Video
                     │
                ┌────┴────┐
                │ ffmpeg  │
                │ whisper │
                └────┬────┘
                     ▼
              🗒 Obsidian Note
          Scan in 5s → dig or skip
```

> 🚫 No MCP server &nbsp; 🚫 No Playwright &nbsp; 🚫 No headless browser
>
> ✅ Just cookies + HTTP + local whisper

## 📂 Output

```
xhs/
├── 📄 2026-03-15 XX的核心发现.md
├── 📄 2026-03-22 YY方法论解析.md
├── 📄 2026-03-29 ZZ技术突破.md
├── 🖼 img/
└── 🎬 video/
```

### 🗒 Note format

```markdown
# One-line insight                     ← judgment, not description

Core argument, 2-3 sentences.

**Relevance:** Why this matters to you.
**Worth digging?** Yes/No + reason.

> [!tip]- Details                       ← 📌 collapsed by default
> Structured content...

> [!info]- Metadata                     ← 📌 collapsed by default
> Source · date · stats · tags
```

## 🚀 Quick start

### 1. Install the plugin

```bash
claude /plugin install chenxiachan/xhs-claude-skills
```

### 2. First run

```
/xhs https://www.xiaohongshu.com/explore/...
```

On first run, the skill will detect no cookies and **guide you through a 30-second setup**:

1. 🌐 Open Chrome → xiaohongshu.com (make sure you're logged in)
2. 🔧 Open DevTools Console (F12)
3. 📋 Paste the snippet the skill gives you → cookies auto-copied
4. 💾 Save to `~/cookies.json`
5. ✅ Done — all future runs use this automatically

> 🔄 When cookies expire, the skill detects it and re-prompts. No manual checking needed.

### 3. Dependencies (video transcription only)

| | Install | Required for |
|:--|:--------|:-------------|
| 🎵 ffmpeg | `brew install ffmpeg` | Audio extraction |
| 🗣 mlx-whisper | `pip install mlx-whisper` | Speech-to-text |

> 💡 Text/image posts work with zero dependencies. Video transcription needs the above.

## ⚙️ Configuration

Edit paths in `skills/xhs/SKILL.md` if your setup differs:

| Setting | Default |
|:--------|:--------|
| 🍪 Cookies | `~/cookies.json` |
| 📁 Output dir | `~/Documents/Obsidian Vault/xhs` |

## 🎨 Personalization

Each note includes a **"Relevance"** line connecting the content to your background. Edit the user description in `skills/xhs/SKILL.md` (step 4) to match your context.

## 📁 Plugin structure

```
rednote-to-obsidian/
├── 📋 .claude-plugin/
│   └── plugin.json
├── 📂 skills/
│   ├── xhs/SKILL.md
│   ├── xhs-batch/SKILL.md
│   └── xhs-analyze/SKILL.md
├── 📄 README.md
└── 📄 README_CN.md
```

<div align="center">

---

MIT License

</div>
