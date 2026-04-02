# Newsreel — Personalized Daily Research Digest

A social-media replacement: high-signal, low-noise daily summaries of research and ideas across curated topics. Designed for interactive exploration — read the digest, then dive deeper into anything interesting.

## How This Project Works

This is a **conversational project**. The user opens it in Codex and can:

1. **Generate today's digest** — "make today's digest" or "what's new today"
2. **Explore articles** — "tell me more about that RNA paper" or "fetch the full text of item 3"
3. **Manage topics** — "add longevity research" or "drop performance psychology" or "make AI higher priority"
4. **Adjust preferences** — "shorter summaries" or "include more Substack" or "add a Spanish-language source"
5. **Review past digests** — "what did we cover on Monday?" or "find all APOE4-relevant items this week"

## Generating a Digest

When asked to create a digest, follow this procedure:

### Step 1: Read Configuration
- Read `config.yaml` for current topics, queries, and preferences
- Read `seen_urls.txt` for dedup

### Step 2: Search
For each topic in `config.yaml`:
1. Run each query with date constraint (e.g., append current year and "last 48 hours" context)
2. Run site-specific searches (substack, hackernews, pubmed, arxiv)
3. Collect all candidate URLs and titles

### Step 3: Dedup
- Check every candidate URL against `seen_urls.txt`
- Skip any URL already seen
- Also skip if the title is substantially similar to a seen article (same story, different outlet)

### Step 4: Filter and Rank
- Apply `personal_context` from config to assess relevance
- Prefer: peer-reviewed research > science journalism > opinion/commentary
- Prefer: novel findings > incremental updates > announcements
- Cap at `max_per_topic` per topic, `max_total` overall
- For high-priority topics, fetch the article content (WebFetch) for richer summaries

### Step 5: Write Digest
- Write to `digests/YYYY-MM-DD.md` following the output style in config
- Each item gets: headline, 3-5 sentence summary, source link
- Add "Personal relevance" callouts where applicable
- End with "Deep Dive Candidates" section for papers worth reading fully
- Include frontmatter with date, topics covered, and article count

### Step 6: Update Seen URLs
- Append all URLs included in today's digest to `seen_urls.txt`
- Format: one URL per line, prefixed with the date: `2026-03-23 https://...`

## Key Files

```
Newsreel/
├── AGENTS.md          ← you are here
├── config.yaml        ← topics, queries, preferences (edit conversationally)
├── seen_urls.txt      ← dedup tracking (auto-managed)
├── digests/           ← daily digest markdown files
│   ├── 2026-03-23.md
│   └── ...
└── .gitignore
```

## Config Changes

When the user asks to change topics or preferences:
1. Read current `config.yaml`
2. Make the requested change
3. Write back the updated file
4. Confirm what changed

## Personal Context

The user is German — APOE4 homozygous, systems thinker, quantified-self practitioner.
This project exists to **replace social media scrolling** with curated, high-value information.
Keep digests scannable (5-10 minute read). Quality over quantity. No filler.

## Style

- Direct, no fluff
- Lead with insight, not headline
- Use programming/systems analogies when explaining research
- Flag APOE4-relevant findings proactively
- Structured output: headers, bullet points, clear grouping
