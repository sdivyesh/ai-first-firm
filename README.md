# Desk and Canon

A memory for a firm, and a record of what it claims. Two places, four one-word verbs, one human action.

This is how I run the written output of AI work at Giant Leap Systems, a 29-person software and data firm in Pune. It is small on purpose. It exists because nothing in an AI thread has a status: the decision the room made and the draft I never finished look the same to the AI, and to the next person who opens the thread.

Built in public, one piece per post. Each post that describes a mechanism adds its files here, so what is described can be installed.

## The pattern

**The Desk** is a folder tree on a shared drive, one folder per project. Working drafts live there. Nothing is deleted; a newer version gets a date in its name. It is the firm's scratchpad and it is allowed to be messy.

**The Canon** is a private repository of plain-text pages: settled decisions, reference pages, case studies, the knowledge base, the firm's story. One `index.md` lists every page and when it was last reviewed. Everyone loads it into whichever AI tool they use, as standing context for their own projects and threads. That is the firm's memory, outside any one product.

**Four verbs** move things between them. The AI tool knows them inside every project.

| Verb | What it does | Approval |
| --- | --- | --- |
| `push` | Takes the file you just downloaded from a chat, finds the project's folder, files it on the Desk | None. Reversible. |
| `promote` | Pushes if needed, decides where in the Canon the page belongs, writes the index row, commits page and index together | One person, one word. The only approval. |
| `checkpoint` | Writes a short note of what the session produced and where it stopped (experimental) | None. |
| `open` | Reads that note back at the start of the next session (experimental) | None. Reads only. |

**The rule**: the AI does every step that can be undone. One person does the one that can't: is this now what the firm claims?

## Install

The skill is written in the Claude skill format (`SKILL.md` with frontmatter). It needs two connectors in the chat: one to the drive that holds the Desk, one to the repository that holds the Canon.

1. Copy `skills/ops-desk/` into your skills folder.
2. Fill in the three locations at the top of `SKILL.md`: the downloads folder, the projects folder, the Canon repository.
3. Create the Canon repository with `templates/index.md` at its root.
4. In each chat project, add one line to its instructions: `Desk slug: <project-name>`.
5. Say `push`.

## What's here

```
skills/ops-desk/SKILL.md   the four verbs
templates/index.md         the Canon index
templates/page.md          a Canon page with its header
posts/                     each post's text, as published, with what it added
```

## Rules the files follow

- The verbs never ask, confirm, draft or suggest. If something looks like it belongs in the Canon, the AI says so in one sentence and stops. `promote` is a person's word.
- Nothing on the Desk is ever deleted.
- Nothing writes to the Canon except `promote`, and `promote` touches only the page and the index.
- Checkpoint and open run only when said. They were tried as automatic steps first and rolled back: every session paid the bookkeeping whether or not the project had moved.

## Status

| Piece | Status |
| --- | --- |
| push, promote | In daily use |
| checkpoint, open | Experimental; rolled back once, back as explicit verbs |
| templates | In use |

## Not here, on purpose

Nothing from our Canon itself. No client work. Nothing about how we do outbound; that is a separate system and it is not public.

## Licence

MIT. Take it.

Divyesh Shah · CTO & Co-founder, Giant Leap Systems, Pune
