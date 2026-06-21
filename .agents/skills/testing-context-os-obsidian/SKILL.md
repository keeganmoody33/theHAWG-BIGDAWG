---
name: testing-context-os-obsidian
description: Test Context OS knowledge graphs in Obsidian. Verifies wiki-link resolution, graph view connectivity, frontmatter validity, and cross-layer navigation. Use when validating Context OS PRs or knowledge graph changes.
---

# Testing Context OS in Obsidian

## When to Use
- After creating or modifying knowledge nodes in a Context OS repo
- After adding/removing wiki-links between nodes
- When validating graph health (orphans, dangling links, connectivity)

## Prerequisites

### Obsidian Installation
Obsidian is not pre-installed. Install via AppImage extraction (FUSE is unavailable on the VM):

```bash
cd /tmp
wget -q "https://github.com/obsidianmd/obsidian-releases/releases/download/v1.8.9/Obsidian-1.8.9.AppImage" -O Obsidian.AppImage
chmod +x Obsidian.AppImage
./Obsidian.AppImage --appimage-extract
# Launch with:
DISPLAY=:0 /tmp/squashfs-root/obsidian --no-sandbox &
```

Note: The version URL might change. Check https://github.com/obsidianmd/obsidian-releases/releases for the latest version if the download fails.

### Python YAML Parser
```bash
pip3 install pyyaml
```

## Devin Secrets Needed
None — this is a local testing workflow with no external services.

## Test Approach

### 1. Programmatic Validation (Shell — No Recording Needed)
Run a Python script to validate:
- All knowledge nodes have valid YAML frontmatter
- Required fields present: `name`, `description`, `domain`, `node_type`, `status`, `last_updated`, `tags`, `topics`, `related_concepts`, `source`
- `domain` matches taxonomy.yaml allowed values
- `node_type` matches taxonomy.yaml allowed values
- `status` is one of: `emergent`, `validated`, `canonical`
- All `[[wiki-links]]` in body text resolve to existing `.md` files
- No orphan nodes (every node has at least 1 outbound + 1 inbound link)
- Link density meets `ontology.yaml` minimums (default: 2 outbound per node)

### 2. Obsidian Visual Verification (Record This)
1. Open vault in Obsidian via "Open folder as vault"
2. Press Ctrl+G to open graph view
3. Verify all nodes appear interconnected (no isolated dots except template placeholders)
4. Click through wiki-links to test navigation between nodes
5. Test cross-layer navigation: 00_foundation doc → knowledge_base node
6. Verify CLAUDE.md navigation table links resolve

### Key Things That Can Go Wrong
- Wiki-links that don't resolve appear as dimmed/unhighlighted text in Obsidian
- Orphan nodes appear as isolated dots in graph view with no connections
- Malformed frontmatter causes Obsidian to not display Properties panel
- Cross-directory wiki-links might not resolve if filenames don't match (Obsidian uses filename-based resolution, not path-based)

### Template Placeholder Nodes
`templates/node_template.md` contains placeholder wiki-links like `[[related-concept-1]]` that will appear as disconnected dots in the graph. This is expected — they are not real knowledge nodes.

## Window Management
Maximize Obsidian before recording:
```bash
sudo apt-get install -y wmctrl
wmctrl -l  # Find Obsidian window ID
wmctrl -i -r <window_id> -b add,maximized_vert,maximized_horz
```
