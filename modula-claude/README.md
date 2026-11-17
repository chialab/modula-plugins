# Modula plugin for Claude

Create and edit interactive lessons, stories, maps and exercises in chat. Describe
what you want to make and provide your text, images and data. Claude drafts the
content with Modula blocks; review it, ask for changes or edit blocks visually in
the Modula editor. When you save or close the editor, Claude presents the result as
a complete HTML document. View it in a browser, reopen it in the Modula editor,
embed its content on your own website, or export a SCORM package for your LMS.

## What it contains

- A connection to the Modula MCP server (`.mcp.json`).
- The `author-content` skill, which tells Claude how to use the Modula tools:
  choose a content structure, design blocks, build a document, edit an existing
  Modula HTML document, and export to SCORM.

The plugin has no hooks, scripts, commands, agents or dependencies, and does not
run any code on your machine.

## Connection and data

The plugin connects over Streamable HTTP to `https://modula.dev/mcp`, the only
service it contacts. You sign in to your own Modula account through the OAuth flow
that Claude starts when you connect. No credentials, tokens or API keys are stored
in this package, and Claude never sees your password.

Claude sends to Modula only what a request needs: the text, data and Modula HTML
documents you ask it to author, edit or export. Modula returns the generated
HTML, scripts and packaging instructions. Nothing is stored on the server as a
document: the result is delivered to the conversation, and publishing it
anywhere is up to you. Available block types and variants depend on your Modula
account. See the [privacy policy](https://modula.dev/privacy) and
[terms](https://modula.dev/terms).

## Use it

After installing the plugin, connect Modula from Claude's connector settings
(`/mcp` in Claude Code) and complete the sign-in. Then ask, for example:

- "Create an interactive lesson about the solar system with a short quiz."
- "Make an interactive timeline of the Roman emperors."
- "Open this Modula HTML file and replace its introduction."
- "Export this lesson as a SCORM 2004 package titled Water cycle."

To try it locally in Claude Code, from the repository root:

```sh
claude --plugin-dir ./apps/app/plugins/modula-claude
```

## Support

Questions and issues: <https://modula.dev/contact>.

## License

MIT, see `LICENSE`. This covers the files of this plugin; use of the Modula
service is governed by its terms.
