# Contradictions

Where conflicting claims across sources live until someone resolves them. This is the single feature that most differentiates a wiki from RAG: contradictions don't get lost in retrieval — they get surfaced and worked.

**Format:**

```
## <short title>

- **Claim A:** <what one source says>
  - Source: [link]
- **Claim B:** <what another source says>
  - Source: [link]
- **Impact:** why this matters (commercial, technical, scope, legal)
- **Owner:** who should resolve
- **Resolution:** ?     ← fill in when resolved; link to the deciding artefact
- **First flagged:** YYYY-MM-DD
```

Claude appends entries during `/ingest` whenever a new source conflicts with existing wiki material. `/lint` lists unresolved entries (`Resolution: ?`) every week.

A contradiction between a `team-inputs/` file (our draft) and a `client-inputs/` file (what the client said) is usually a **discovery finding** — treat it as work, not a bug.

---

<!-- Newest at the bottom. Resolved entries stay as the audit trail. -->
