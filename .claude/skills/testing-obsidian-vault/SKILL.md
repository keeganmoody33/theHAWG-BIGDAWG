---
name: testing-obsidian-vault
description: Test the theHAWG-BIGDAWG Context OS knowledge graph in Obsidian. Verifies wiki-link rendering, node frontmatter, graph connectivity, and cross-layer navigation. Use when verifying knowledge node changes, enrichment updates, or graph structure modifications.
---

# Testing theHAWG-BIGDAWG in Obsidian

## Prerequisites

- Obsidian AppImage (v1.8.9+) — may need to extract with `--appimage-extract` if FUSE is unavailable
- The repo cloned with `.obsidian/` config directory present (graph.json, workspace.json, core-plugins.json)

## Devin Secrets Needed

None required for Obsidian testing. If testing API enrichment data accuracy, you need:
- `THEHOG_ACCESS_KEY` (ak_ prefix)
- `THEHOG_SECRET_KEY` (sk_ prefix)

## Setup

### Installing Obsidian

```bash
cd /home/ubuntu
curl -sL "https://github.com/obsidianmd/obsidian-releases/releases/download/v1.8.9/Obsidian-1.8.9.AppImage" -o Obsidian.AppImage
chmod +x Obsidian.AppImage

# If FUSE is not available (common on VMs), extract instead:
./Obsidian.AppImage --appimage-extract > /dev/null 2>&1

# Launch from extracted directory:
DISPLAY=:0 ./squashfs-root/obsidian --no-sandbox --disable-gpu &
```

### Opening the Vault

The repo IS the vault. Obsidian should auto-detect it if `.obsidian/` config exists from a previous session. If not:
1. Open Obsidian
2. Click "Open folder as vault"
3. Select `/home/ubuntu/repos/theHAWG-BIGDAWG/`

### Maximizing for Recording

```bash
sudo apt-get install -y wmctrl 2>/dev/null
wmctrl -r :ACTIVE: -b add,maximized_vert,maximized_horz
```

## Test Patterns

### 1. Node Content Verification

Navigate to a node in the sidebar (knowledge_base/ tree). Check:
- **Frontmatter renders as Properties table** — name, description, domain, tags, topics, related_concepts, source
- **Wiki-links render as clickable links** — `[[concept-name]]` should be blue/clickable, not plain text
- **Evidence blocks render as blockquotes** — `>` lines with `[VERIFIED: ...]` attribution tags
- **Sections render correctly** — Background, Role, Evidence, How It Relates, Status

### 2. Wiki-Link Navigation

Click a wiki-link in the "How It Relates" section or body text. Verify:
- It navigates to the target node (not a "create new note" prompt)
- The target node renders its own frontmatter and content correctly
- Back navigation works (Ctrl+Alt+Left or browser-style back button)

### 3. Graph View (Ctrl+G)

Open the graph view and verify:
- All knowledge nodes appear as dots
- Hub nodes (gtm-engineering, thehog-ai-company, thehog-ai-api) are visually larger/more connected
- No orphan nodes (isolated dots with no edges)
- Person nodes (hudson-liao, paulo-nascimento) connect to thehog-ai-company

### 4. Cross-Layer Navigation

Test links from `00_foundation/` docs to `knowledge_base/` nodes:
- Open an operational doc (e.g., icp-scoring-engine.md)
- Click a `[[wiki-link]]` to a knowledge node
- Verify it resolves correctly across the directory boundary

## Common Issues

- **FUSE not available**: Extract AppImage with `--appimage-extract` and run from `squashfs-root/obsidian`
- **Obsidian doesn't open the vault**: Check if `~/.config/obsidian` remembers the vault path; may need to re-open manually
- **Frontmatter not rendering as table**: Ensure the file starts with `---` YAML frontmatter block. Obsidian renders this as a Properties panel in Reading/Live Preview mode
- **Wiki-links showing as plain text**: Verify the format is `[[name]]` not `[name]` or `(name)`. Obsidian requires double brackets
- **Graph view missing nodes**: Some nodes may be filtered out. Check Filters panel in graph view — ensure no path filters are excluding directories

## What to Verify Per Change Type

| Change Type | Key Assertions |
|---|---|
| New knowledge node | Frontmatter valid, 5+ outbound wiki-links, appears in graph, no orphan |
| Node enrichment | New fields render (social, press), attribution tags present, status updated |
| Wiki-link updates | All links resolve (no "create new note"), bidirectional where expected |
| Operational doc change | Cross-layer links still resolve to knowledge nodes |
| Graph structure change | Graph view shows expected connectivity, no new orphans |
