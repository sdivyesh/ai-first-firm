---
name: "ops-desk"
description: Four one-word verbs for moving work between an AI chat, a shared drive (the Desk) and a plain-text repository (the Canon). "push" files the document just produced into its project folder on the Desk. "promote" pushes if needed, then commits the document to the Canon and updates index.md in the same commit; it is the only verb that writes to the Canon and it runs only on that word. "checkpoint" and "open" (experimental) write and read a short note of where a project's work stopped. Trigger on the bare word, with or without a file named. Never ask, confirm, or propose.
---

# ops-desk — push, promote, checkpoint, open

Two places. Four verbs. Output is one line.

## Fill in before use

```
DOWNLOADS   <drive folder id>   where the browser saves what you download from a chat
PROJECTS    <drive folder id>   one sub-folder per project, created on first use
CANON       <owner>/<repo>, branch <main>   the plain-text record; index.md at its root
```

Everything under the drive folders is found by title. The Canon is addressed by repo path.

## Which project

The chat project's instructions carry a line `Desk slug: <slug>`. If absent, slug = the project's name, kebab-cased. "push to <slug>" overrides for one call.

## Which file

The normal loop is: download the document from the chat (it lands in DOWNLOADS) → say "push". So, in order:

1. The file named, looked up in DOWNLOADS by title.
2. Else the newest file in DOWNLOADS.
3. Only if DOWNLOADS is empty and no file is named: the deliverable most recently produced in this session. If it exists only as chat text, write it to a file first. Markdown uploads as text/markdown, not converted to the drive's native document type.

If a file is named but is not in DOWNLOADS, say so in one line and stop; the drive may still be syncing. Never regenerate a file that was meant to come from DOWNLOADS.

## push

1. Find `PROJECTS/<slug>/` by title. Not found → create it.
2. If a file of the same name is already there, rename the incoming file with the date (`name 2026-11-07.ext`). Nothing on the Desk is ever deleted.
3. File came from DOWNLOADS → move it (keep its id). File came from this session → create it in the folder.
4. Say: `pushed · <slug>/<file>`

No approval. Everything push does can be undone.

## promote

1. If the file is not yet on the Desk, push it (silently).
2. Text only: `.md`, `.html`, `.txt`, `.json`, `.csv`. Any other type: say `not promoted · <file> is binary` and stop. The push in step 1 stands.
3. Decide the destination path from what the file is: a firm-level reference page → `<file>`; a finished deliverable → `assets/<file>`; a page about something built → `kb/<file>`. If the user names a sub-folder, use it.
4. Read `index.md` and the destination path if it already exists.
5. Edit `index.md`: add or replace the file's row in the format of its neighbours, with today's date as last reviewed.
6. Commit once, to the Canon branch: the file at its destination path and the edited `index.md`. Message: `promote: <path>`. A file at the same path is replaced; the repo holds the history.
7. Say: `promoted · <path>`

This is the one place a person decides. The verb runs only on the word "promote", from the user, in the chat. Nothing else writes to the Canon.

## checkpoint (experimental)

On the word "checkpoint", at the end of a session:

1. Write `PROJECTS/<slug>/CHECKPOINT.md`, replacing any existing one. At most twelve lines: today's date; what was produced this session (files pushed or promoted, by name); where the work stopped, in one sentence; open items, one line each.
2. Say: `checkpoint · <slug> · <date>`

Only on the word. Never at the end of a session unasked. Never a summary of the chat; a note of where to resume.

## open (experimental)

On the word "open", at the start of a session:

1. Read `PROJECTS/<slug>/CHECKPOINT.md`. Not found → say `open · <slug> · no checkpoint` and stop.
2. Say, in three lines: when the last session was; what it produced; where it stopped. Then the open items, if any.

Reads only. Never writes, never acts on the open items.

### Status of checkpoint and open

Tried once and rolled back. The cost was that every session ended with a checkpoint and started with an open, whether or not the project had moved, and the bookkeeping outweighed the recall. They are back as verbs you say, not steps that run on their own, which is the whole difference. Still experimenting.

## Rules

- The verbs never ask, confirm, draft, or suggest. If something looks like it belongs in the Canon, say so in one sentence and stop; "promote" is the user's word.
- Never write project state anywhere except CHECKPOINT.md, and only on "checkpoint".
- Never edit or delete anything in the Canon other than the promoted file and `index.md`.
- Load the drive tools and the repo tools once, at the first verb, not per step. If the repo tools are not available, `promote` says `not promoted · repo connector is off` and stops; the other verbs are unaffected.
