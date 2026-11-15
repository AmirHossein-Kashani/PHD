---
type: guide
tags: [type/guide]
---
# How to Use This Vault

> [!summary] In short
> Open this folder in **Obsidian** as a vault. Start from [[00 Home]]. Every note links to its **project** and its **topics**, so the graph view shows how everything is connected. New work goes into a **daily note**, a **meeting note** or a **paper note** made from the templates.

## 1. Open it

1. Install Obsidian (free): https://obsidian.md
2. Unzip the folder `PhD Research Vault`.
3. In Obsidian: **Open folder as vault** and choose `PhD Research Vault`.
4. Open [[00 Home]]. (Tip: right-click it and choose "Bookmark", so it is always one click away.)

## 2. How notes are connected

Every note made from Notion starts with the same block:

- **In short** - one or two lines in simple English.
- **Project** - links to the project hub(s).
- **Topics** - links to the topic hub(s).
- **Part of / Sub-notes** - parent and child pages (same as in Notion).
- **From meeting** - for reports: a link to the dated meeting entry where the report was shown.
- **Original** - link to the Notion page (for files and images that are only in Notion).

The note properties at the top (`type`, `date`, `status`, `projects`, `topics`, `tags`) are the same for all notes, so you can search and filter.

## 3. Graph view

- Open **Graph view** (left bar, or `Ctrl/Cmd + G`).
- The graph has **no meeting or day nodes**. It only shows projects, topics, papers, reports and learning notes. Colors are already set by type.
- To see one project only, type in the graph filter: `tag:#project/auto-researcher` or `tag:#project/multi-source-rag`.
- To hide the Notion category pages (they connect to many notes): `-path:"6 Notion Categories"`.
- Use **Local graph** (right-click a note, "Open local graph") to see only the neighbors of one note.

## 4. Where meetings and days are

- A meeting is **not a note**. Its content is split by subject and stored as a dated entry (`### 2026-10-14 One-on-One`) inside:
  - [[AR 7 - Meeting Log]] and [[MS - Meeting Log]] for the two active projects,
  - the "Notes from meetings" section of the older project hubs and of the topic notes.
- Each entry has a link to the original Notion page and links to the other parts of the same meeting.
- Daily plans are in [[01 Now and Next#Daily log]].
- To find everything from one meeting, search its date, for example `2026-10-14`.

## 5. Useful searches

| Goal | Type this in Search |
|---|---|
| Open tasks with a word | `task-todo:baseline` (any word after the colon) |
| All papers of a project | `tag:#type/paper tag:#project/auto-researcher` |
| Everything from October meetings | `"### 2026-10"` |
| A word in all notes | just type the word |

## 6. Adding new content

Turn on the **Templates** core plugin (Settings > Core plugins > Templates) and set the template folder to `_templates`. Then use:

| Template | Use it for | Where it goes |
|---|---|---|
| `Meeting entry` | a one-on-one or group meeting | insert it at the top of the entries in [[AR 7 - Meeting Log]], [[MS - Meeting Log]] or the topic note it is about (no new note) |
| `Daily entry` | your plan for the day | insert it at the top of [[01 Now and Next#Daily log]] (no new note) |
| `Paper` | one paper you read | new note, short paper name, in `3 Papers`. A research-agent system for the Auto Researcher related work goes in `1 Projects/Auto Researcher/AR 4 - Related Work/<group>` instead |
| `Experiment` | one experiment run and its results | new note, short name (no date in the name), in `4 Reports` |
| `Idea` | a bigger idea (or just add a row to [[03 Ideas Inbox]]) | new note, short idea name |

Rules that keep the graph clean:
1. **Notes are about things (projects, topics, papers, experiments), not about dates.** Put dates inside notes, not in note names.
2. **Always link the project and at least one topic** in a new note.
3. **Write the "In short" line** first. One or two simple sentences.
4. Put decisions in [[04 Decisions Log]] and ideas in [[03 Ideas Inbox]].
5. Update [[01 Now and Next]] after each meeting.

## 7. Suggested plugins (optional)

- **Dataview** - make tables from note properties (for example all papers with `status: Done`).
- **Excalidraw** - draw architecture diagrams inside the vault.

## 8. What is not inside

- **PDF, Word and other files** stayed in Notion. The list with links is in [[07 Files Index]]. You can download them from Notion and drop them into `attachments`.
- **Images** were downloaded and are in `attachments` (41 images).
- Notion comments and page history are not included.
