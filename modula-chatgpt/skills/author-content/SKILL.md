---
name: author-content
description: Use when the user wants to create, review or edit interactive lessons, stories, maps, exercises or presentations in chat with Modula blocks, or save, reopen and export a Modula HTML document.
---

# Author content with Modula

Use the connected Modula MCP server for block authoring and export. Follow its
current tool schemas and returned instructions; do not guess block properties or
copy schemas from previous sessions. If the tools are unavailable, explain that
the plugin needs its Modula connection and OAuth sign-in before continuing.

## Create and edit blocks

Create a Modula document by default, for a single block too. Author separate
fragments only when the user explicitly requests a custom page layout or
embedding.

1. For several blocks, call `get_schema_instructions` first. Choose `lesson`,
   `presentation`, `documentary`, `geostory`, or `assessment` for the requested
   content, and `none` when none fits. These are internal instruction choices;
   describe the result to the user as their lesson, story, presentation, or
   document. A single requested block skips this step and gets a document of
   that block alone, without extra blocks.
2. Follow the returned instructions. Use `list_types` when you need to discover
   available block types and variants. Respect the account's availability and
   choose an available alternative when a type or variant is refused.
3. Design the document with `design_document`: a title and its blocks (types,
   variants, and panel or story children) in reading order. When creating a
   document, consider passing a `theme`: a single accent colour suited to the
   content or the user's palette can be enough. The result names the document
   by a `document` id and gives the id and the exact data schema of each block.
   Modula keeps the document: pass that id with later calls, never the
   document itself. To change the structure of an existing block, include its
   id in `design_document`; to move or remove blocks, pass `order`.
4. Open the document at once with `edit_document`, then write the blocks in
   order with `write_document`, `{ replace: id, data }` for each, a few per
   call, each call as soon as its blocks are ready; never hold them all for the
   end. The user sees the document take shape. Data that is ready can go in
   `design_document` at once (a single block: design it with its data, then
   open it). To edit fields of a written block, pass
   `{ replace: id, data: changedFields }`; supply changed arrays in full. If a
   change is answered with the current data of a block (the user edited it),
   apply your change to that data and call again.
5. The editor saves what the user does as they do it, and the user takes back
   any change, yours included, with undo. Your context says what the user
   changed there; `get_document` with `format: 'data'` reads their current
   data. When the user asks you for a change from the editor, it stays open:
   change the document with `write_document`, do not open it again. Where the
   host allows it, the user downloads the file from the editor and no message
   follows. When a message asks you for the file, call `get_document` and
   present the file as it is, as an artifact if the host supports them,
   otherwise as a file.
6. Only for a custom page layout, call `get_document` with `format: 'page'` and
   your `layout`: your HTML around the blocks, with `<!-- modula:blocks -->`
   where they go. It attaches the page in one file, its script inline, to
   deliver as it is. To embed the blocks in a site the user hosts, use
   `format: 'fragments'` and follow its instructions for the script. To change
   a block, change the document and ask again. Never present on its own a
   block of a larger page.

For a learning management system, call `get_document` with `format: 'scorm'`
and `title` (and `masteryScore` when asked): it attaches the package, a zip
to upload as it is. Never copy HTML out of a document, hand-write SCORM files
or build the zip yourself.

Use `search` for media (with their credits) and for the coordinates of places,
instead of guessing a URL or a position.

Use `get_theming_instructions` for requested styling. `load_document`,
`save_document` and `get_editor_options` are called by the editor widget, not by the model.

## Open and deliver documents

- To edit a Modula document the user attaches, read the supplied HTML file
  through the host's file tools (or another connector) and pass it once to
  `edit_document` as `html` (the whole file, or its head up to `</head>`); where
  no editor shows, pass it to `write_document`. The tools accept HTML text, not
  a path or URL. The result names the document by a `document` id: use that id
  from then on.
- To deliver a document, call `get_document` with its id: it attaches the
  complete file, to save or deliver as it is.
- To put blocks of a document in a page of your own, call `get_document` with
  `format: 'page'` and your layout (and `blocks` for some of them); to embed
  them in a site, `format: 'fragments'`. Never copy HTML out of a file.
- Modula keeps a working copy of a document for a day after its last use, a
  draft and not a hosted document: the file in the chat is what the user keeps.
  Before a turn that changed a document ends, or when your context says the
  file of the chat is behind, call `get_document` and deliver the file. When a
  call answers that a document is no longer available, pass its latest file
  again as `html`, or ask the user to attach it. Report a saved file or published
  page only after the corresponding action succeeds.

Use `submit_support_ticket` only when the user asks to contact Modula support;
include the relevant issue and avoid credentials or unrelated personal data.
