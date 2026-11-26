# Modula plugin for ChatGPT

Create and edit interactive lessons, stories, maps and exercises in chat. Describe
what you want to make and provide your text, images and data. Follow the document
as the assistant writes it in the Modula editor, edit blocks visually, and use undo
to take back changes. Save the latest version in your conversation as a complete
HTML file, as an artifact where the host supports it. Reopen the file in Modula,
publish it on your own website, embed its blocks, or export a SCORM 2004 package
for your LMS.

## Package

- `plugin.json`: listing, review cases and publication metadata.
- `mcp.json`: the remote Modula connection.
- `skills/author-content/SKILL.md`: authoring and delivery instructions.
- `assets/`: icons; `LICENSE`: the license for this plugin package.

Import this directory through a host that supports Agent Plugins 1.0. If an
archive is required, put the contents of this directory at the archive root,
including its manifests, skill and referenced icons. Do not include credentials,
reviewer access details, preparation notes, dependency folders or generated app
bundles. Review cases describe expected behavior; they are not test results.

## Connection and data

The plugin connects to `https://modula.dev/mcp` over Streamable HTTP. Complete
OAuth sign-in with your own Modula account. The ChatGPT configuration uses the
registered public client `client_chatgpt`, without a client secret, and the
`modula:mcp:tools` scope. Available blocks depend on your account.

Modula keeps a temporary, encrypted working copy for one day after its last use.
The HTML file saved in your conversation is the lasting copy. The plugin does not
host your document or publish a website for you.

## Try it

- Create an interactive lesson about the solar system with a short quiz.
- Make an interactive timeline of the Roman emperors.
- Open this Modula HTML file and replace its introduction.
- Export this lesson as a SCORM 2004 package titled Water cycle.

## Support and policies

[Modula](https://modula.dev) · [Support](https://modula.dev/contact) ·
[Privacy](https://modula.dev/privacy) · [Terms](https://modula.dev/terms).

The Claude package lives in `../modula-claude` and shares the authoring skill and
branding. The package license covers these plugin files; the Modula service has
its own terms.
