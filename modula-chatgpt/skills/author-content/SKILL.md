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
3. Call `design_blocks` with the types, variants, and panel children for a section
   in reading order. For revisions, include the existing block ids in the design.
4. For a document, pass the design tokens and data directly to `edit_document`
   with a title and `insert` changes (and `isNew` for content the user has not
   seen yet); do not write each block with `edit_block` first. The document
   opens in the editor widget: Save and close, Reject (with `isNew`) or Cancel.
   A message from the user follows: then present the latest file as it is, as
   an artifact if the host supports them, otherwise as a file; after a
   rejection, drop it and ask what to change. Retain the returned source for
   later changes.
5. Only for a custom page or embedding, write one block per call, never several,
   with that block's token and data matching its exact schema: the first block
   with `review_block`, which opens it in the editor widget, the others with
   `edit_block`, which opens none. To review several blocks, open them one at a
   time with `review_block` and `pendingReviews` (the blocks you open after it):
   the message after each save or close asks for the next. Put the returned
   HTML fragment in the page and keep the returned block metadata beside it,
   with its updated token; inspect validation reports before proceeding. The
   latest token of a block rewrites it; call `design_blocks` with its id only to
   change its structure. Never present on its own a block of a larger page.
6. When the custom page is done, call `get_script` with the latest token of every
   block in it and follow its delivery instructions: the page loads that one
   script.

For a learning management system, use the latest fragments and tokens. When
starting from a document, get them with `extract_blocks` using its latest source.
Build the package page from those fragments and call `export_scorm` with every
latest block token and `title` (and `masteryScore` when asked), following its
packaging instructions. Never copy HTML out of a document or hand-write SCORM files.

Use `get_theming_instructions` for requested styling. `save_document` is called
by the editor widget, not by the model.

## Open and deliver documents

- To edit an existing Modula document, read the supplied HTML file through the
  host's file tools (or another connector) and pass its source (the head of the
  file, up to `</head>`) to `edit_document` with the changes (`replace`,
  `insert`, `order`). The tool accepts HTML text, not a path or URL. It returns
  the complete updated file as an attached resource, to save or deliver as it
  is, and the new source in its text for the next change. Without changes it
  opens the document in the editor.
- To create a new document, call `edit_document` with a title and `insert`
  changes.
- To use blocks of a document in another page, pass its source to
  `extract_blocks` (with `ids` for some of them): it returns each block's
  fragment and metadata with its token, for `get_script`. Never copy HTML out
  of a file.
- `edit_document` returns file contents; it does not write to disk or persist a
  document on the server. Report a saved file or published page only after the
  corresponding action succeeds.

Use `submit_support_ticket` only when the user asks to contact Modula support;
include the relevant issue and avoid credentials or unrelated personal data.
