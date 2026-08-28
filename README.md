# jslkon-docs

Content source for the `/documents` section of
[jslkon-dashboard-fe](https://github.com/jslKon/jslkon-dashboard-fe).

Consumed by Fumadocs at **build time**: the dashboard's Docker build clones this repo into
`./content`, and `next build` compiles every file below into a static page. Publishing a doc is
a push here plus a redeploy of the dashboard.

> **Status: placeholder.** Two sections exist so the layout, sidebar and diagram rendering can
> be reviewed. The prose is deliberately throwaway — replace it before publishing.

## Layout

The file path *is* the URL and *is* the sidebar tree. No registry to maintain.

```
content/docs/index.md                   →  /documents
content/docs/kafka/index.md             →  /documents/kafka
content/docs/kafka/partitions.md        →  /documents/kafka/partitions
```

`index.md` is a folder's landing page. `meta.json` controls sidebar ordering and folder labels.

## Frontmatter

```yaml
---
title: Partitions and Consumer Groups   # required — page title and sidebar label
description: One-line summary.          # required — shown in search results
tags: [kafka, messaging]                # optional — drives tag pages and search
date: 2026-08-28                        # optional — YYYY-MM-DD
---
```

`title` and `description` are validated by the Zod schema in the dashboard's
`source.config.ts`. A typo in a key fails the build rather than silently disappearing.

## Diagrams

PlantUML is rendered by the dashboard's `/api/documents/diagram` route. Two forms work:

- **Inline** — a fenced ` ```plantuml ` block (see `design-patterns/creational/singleton.md`)
- **Standalone** — a `.puml` file beside the doc, referenced with image syntax:
  `![Kafka cluster topology](kafka.puml)` (see `kafka/partitions.md`)
