# Patent Downloader Skill

A Claude Code skill for high-precision patent file acquisition. Triggered by upstream analysis skills to download full-text PDFs and patent drawings for specific high-priority patents, then hands off to `patent-structured-analysis` for deep reading.

## Five-Skill Pipeline

This skill is the **targeted download bridge** between landscape analysis and single-patent deep-dive.

```
[1] pro-patent-search          [2] patent-mapping              [3] patent-deployment
   Search & score         →      9 strategy charts        →      Filing strategy
                                        │                               │
                              Identify key patents           Specify analysis targets
                                        └──────────┬─────────────────┘
                                                   ▼
                                     [4] patent-downloader (this skill)
                                         Download PDF + drawings
                                                   │
                                                   ▼
                                     [5] patent-structured-analysis
                                         Claim tree · FTO · Design-around
```

**What triggers this skill:**

| Upstream signal | Trigger scenario | What to download |
|----------------|-----------------|-----------------|
| patent-mapping — citation network | High-citation pioneer patent needs deep analysis | Root-node patent PDF |
| patent-mapping — competitor radar | Competitor's strong IPC dimension needs claim scope analysis | Competitor core patent PDF |
| patent-deployment — Choke Point | Need exact claim scope of competitor's critical-path patent | Competitor core patent PDF |
| patent-deployment — Stronghold | Invalidity search: confirm Blue Ocean cell has no prior art | Prior art patent PDFs |
| patent-deployment — Fence/Cluster | Confirm peripheral branch claim scope before filing | Peripheral technology PDFs |

**After download:**
Pass PDF absolute path and drawings folder to `patent-structured-analysis` for structured deep analysis.

## Capabilities

- **Direct PDF Fetching** — automatic retrieval of patent PDFs with metadata enrichment
- **Visual Asset Extraction** — automatic extraction of patent drawings and images, organized by figure number
- **Self-Healing** — automatic fallback to Playwright browser rendering if static API is blocked

## Usage

```powershell
# Single patent download
python scripts/google_patents_collector.py --query "US11311692B2" --enrich --max 1 --no-tor

# Batch download by assignee
python scripts/google_patents_collector.py --query "assignee:Lunatech" --enrich --max 20 --no-tor
```

Files are organized automatically into `./downloads/{Assignee}/{Patent_ID}_{Title_Slug}.pdf`.

## Triggers

- 「下載專利 [專利號] 的全文」
- 「幫我把這 10 篇專利的 PDF 都抓下來」
- 「獲取專利 [專利號] 的所有附圖」
- 「下載 [公司名] 最近三年的所有專利文件」

## Skill SOP

See `SKILL.md` for the full download workflow and error handling (403/503 fallback, Espacenet redirect).