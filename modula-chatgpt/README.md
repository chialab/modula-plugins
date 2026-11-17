# Modula plugin for ChatGPT

Create and edit interactive lessons, stories, maps and exercises in chat. Describe
what you want to make and provide your text, images and data. Your assistant drafts
the content with Modula blocks; review it, ask for changes or edit blocks visually.
When you save or close the editor, your assistant presents the result as a complete
HTML document, as an artifact where your chat app supports it. View it in a browser,
reopen it in the Modula editor, publish it or embed its content on your own website,
or export a SCORM package for your LMS.

This is the Agent Plugins 1.0 package for ChatGPT and compatible hosts. The Claude
package is in [`../modula-claude`](../modula-claude).

## What it contains

- `plugin.json` and `mcp.json`: the Agent Plugins 1.0 manifests.
- `skills/author-content`: the skill that tells the assistant how to use the Modula
  tools: choose a content structure, design blocks, build and edit a document,
  and export to SCORM.
- `assets/`: branding.

The plugin has no scripts or dependencies and does not run code on your machine.

## Connection and data

The plugin connects over Streamable HTTP to `https://modula.dev/mcp`, the only
service it contacts, with the `modula:mcp:tools` scope. You sign in to your own
Modula account through the host's connection flow. The manifest uses a public OAuth
client without a client secret; no credentials belong in this package.

The assistant sends to Modula only what a request needs: the text, data and Modula
HTML documents you ask it to author, edit or export. Available block types and
variants depend on your Modula account. See the
[privacy policy](https://modula.dev/privacy) and [terms](https://modula.dev/terms).

## Self-hosting

For a self-hosted deployment, change `mcpServers.modula.url` in `mcp.json` to your
`/mcp` endpoint, which must be reachable by the host.

## Support

Questions and issues: <https://modula.dev/contact>.

## License

MIT, see `LICENSE`. This covers the files of this plugin; use of the Modula service
is governed by its terms.
