---
description: "Compound Refresh — Knowledge Maintenance. Review existing knowledge notes against the current codebase and team state. Keep the knowledge base accurate, connected, and useful."
user_invocable: true
---

# Compound Refresh — Knowledge Maintenance

Review existing notes in your knowledge base against the current codebase and team state. Keep the knowledge base accurate, connected, and useful.

## When to use
- Monthly maintenance (pair with `/ideate`)
- After a major refactor that may have invalidated old notes
- When a note leads someone astray
- When your knowledge index feels stale

## Instructions

### Phase 1: Inventory

Count notes, list by type, and check if the index is in sync with actual note files.

Flag if index count != actual note count (index out of sync).

### Phase 2: Assess Each Note

For each note, check:

1. **Do the referenced files/paths still exist?** (glob/grep for paths mentioned)
2. **Is the insight still accurate?** (has the underlying code/infra changed?)
3. **Are the tags still correct?** (would someone find this with obvious searches?)
4. **Are links still valid?** (do linked notes still exist and are they still relevant?)
5. **Is status correct?** (should any `active` notes be `superseded` or `archived`?)

### Phase 3: Classify

| Action | When | What to do |
|--------|------|-----------|
| **Keep** | Still accurate and useful | Nothing — leave as-is |
| **Update** | Mostly right but details changed | Edit the note with current info |
| **Consolidate** | Multiple notes cover the same insight | Merge into one atomic note, mark others `superseded` |
| **Relink** | Connections are missing or wrong | Update `links:` arrays + relationship graph |
| **Supersede** | Fundamentally wrong but topic matters | Write new note, mark old as `status: superseded` |
| **Archive** | No longer relevant | Set `status: archived` (don't delete — git history is backup, but status field is faster to filter) |

### Phase 4: Heal the Graph

After individual note assessment:

1. **Orphan check:** Find notes with zero links — should they be connected to something?
2. **Cluster check:** Are there groups of related notes that should link to each other?
3. **Contradiction check:** Are there notes with `contradicts` relationships that need resolution?
4. **Rebuild index** if it's out of sync — regenerate from actual note frontmatter
5. **Rebuild relationship graph** if links have changed — regenerate from note `links:` arrays

### Phase 5: Execute

- **Prefer Keep** — only change notes when there's clear evidence they're wrong
- For Update and Consolidate, show the diff before writing
- Ask the user before any Supersede or Archive action
- When consolidating, keep the most complete note and merge content into it

### Phase 6: Report

```markdown
## Compound Refresh Report — YYYY-MM-DD

### Inventory
- **Total notes:** N
- **By type:** solution: N, research: N, decision: N, pattern: N, playbook: N
- **Active:** N | Superseded: N | Archived: N

### Changes
- **Kept:** N (unchanged)
- **Updated:** N (list titles)
- **Consolidated:** N → N (list merges)
- **Relinked:** N (new connections added)
- **Superseded:** N (list titles + reason)
- **Archived:** N (list titles + reason)

### Health
- **Orphan notes (no links):** N
- **Index sync:** ✓/✗
- **Graph sync:** ✓/✗
- **Contradictions unresolved:** N

### Next refresh: YYYY-MM-DD
```

## Rules
- Evidence informs judgment — don't edit notes mechanically
- Match notes to reality, not the reverse
- When in doubt, Keep — false deletions lose knowledge
- One refresh per session max — don't burn context on maintenance
- Prefer marking `superseded` over deleting — keeps the learning trail visible
- Always rebuild index and graph if any notes changed
