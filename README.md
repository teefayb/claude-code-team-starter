# Claude Code team setup starter

Two files you commit once so everyone who clones your repo gets the same
Claude Code setup:

| File | What it does |
|---|---|
| `CLAUDE.md` | Project instructions Claude Code loads at the start of every session: commands, conventions, do's and don'ts. |
| `.mcp.json` | The project's MCP servers (extra tools for Claude). Every teammate gets the same list and approves it once. |

Plus two optional extras:

| File | What it does |
|---|---|
| `.claude/settings.json` | Shared permission rules: what Claude may run without asking, what it must ask about, and what it can never touch. |
| `.gitignore` | Keeps each person's own `CLAUDE.local.md` and `.claude/settings.local.json` out of git, along with `.env` files. |

No API keys in any of them. Secrets stay in each person's environment.

## Why bother

Without these files every developer sets Claude Code up by hand, and each one
gets different results. With them committed:

- Claude knows your test command, your folder layout and your rules from the first prompt.
- New teammates get the same MCP tools by cloning the repo.
- Rules change through pull requests, like the rest of your code.

## Quick start (60 seconds)

```bash
# 1. From the root of your project, copy the files in
git clone --depth 1 <REPO_URL> /tmp/cc-starter
cp /tmp/cc-starter/CLAUDE.md /tmp/cc-starter/.mcp.json .
mkdir -p .claude && cp /tmp/cc-starter/.claude/settings.json .claude/
cat /tmp/cc-starter/.gitignore >> .gitignore

# 2. Fill in the <PLACEHOLDERS> in CLAUDE.md and delete what you don't need

# 3. Each teammate sets the token the GitHub MCP server needs, in their own shell
export GITHUB_PAT=...   # a fine-grained GitHub token; never commit it

# 4. Commit and start Claude Code
git add CLAUDE.md .mcp.json .claude/settings.json .gitignore
git commit -m "Add shared Claude Code setup"
claude
```

On first launch Claude Code asks each person to approve the servers in
`.mcp.json` and to trust the folder (which applies the `allow` rules in
`.claude/settings.json`). Inside a session, `/mcp` shows each server's status,
and `/context` lists the instruction files that loaded.

Shopify theme? Copy the files from [`examples/shopify-theme/`](examples/shopify-theme/) instead.

## Customize `CLAUDE.md`

- **Keep it short.** Anthropic's guidance is under 200 lines per file. Every line
  costs context in every session.
- **Be specific enough to check.** "Run `npm test` before committing" works.
  "Test your changes" doesn't.
- **Only write what Claude can't see in the code.** Skip anything it can work out
  from `package.json` or the folder names.
- **Comment freely.** Block-level HTML comments (`<!-- like this -->`) are stripped
  before the file reaches Claude, so the template's notes cost nothing.
- **Personal notes** go in `CLAUDE.local.md` next to it. It loads after
  `CLAUDE.md` and stays out of git.
- **Bigger projects** can split rules into `.claude/rules/*.md` files, or import
  other files with `@path/to/file.md`.
- **Don't want to start from scratch?** Run `/init` in Claude Code to draft a
  `CLAUDE.md` from your codebase, then trim it.

`CLAUDE.md` is guidance, not enforcement. For hard limits, use the permission
rules in `.claude/settings.json`.

## Customize `.mcp.json`

The template ships one server: GitHub's hosted MCP server (issues, pull
requests, code search). Its token is referenced as `${GITHUB_PAT}`, which
Claude Code expands from each person's environment when it loads the file.

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": { "Authorization": "Bearer ${GITHUB_PAT}" }
    }
  }
}
```

- **Add a server** by adding an entry under `mcpServers`, or run
  `claude mcp add --scope project ...` and Claude Code writes it here for you.
- **Remote servers** use `"type": "http"` with a `url`. **Local servers** use
  `"type": "stdio"` with a `command` and `args`.
- **Secrets** go in `${VAR}` references (`${VAR:-default}` sets a fallback).
  Expansion works in `command`, `args`, `env`, `url` and `headers`. If a variable
  isn't set, the server still loads but `claude mcp list` shows a warning.
- **Approvals:** Claude Code asks before using servers from a project's
  `.mcp.json`. To ask again, run `claude mcp reset-project-choices`.
- **Same name in two places?** Your local config beats the project file, which
  beats your user config.

Only add servers you'd trust to run on every teammate's machine.

Came from the video? The demo there uses the filesystem MCP server because it
needs no setup. Claude Code already has built-in file tools, so this template
ships a server that adds something new instead.

## Customize `.claude/settings.json`

```json
"allow": ["Bash(npm run lint)", "Bash(npm run test *)", ...],
"ask":   ["Bash(git push *)"],
"deny":  ["Read(./.env)", "Read(./.env.*)", "Read(./secrets/**)"]
```

- Rules are checked in order: **deny, then ask, then allow**. A deny always wins.
- `Bash(npm run test *)` matches `npm run test` with or without arguments.
- `allow` rules only take effect after each person trusts the folder. `deny`
  and `ask` apply straight away.
- Settings files are strict JSON: no comments, no trailing commas.
- Personal tweaks go in `.claude/settings.local.json`, which overrides this file
  and stays out of git.

## Example: Shopify theme

[`examples/shopify-theme/`](examples/shopify-theme/) is a ready-made setup for an
Online Store 2.0 theme:

- **`CLAUDE.md`**: Shopify CLI commands (`shopify theme dev`, `shopify theme check`),
  where sections, blocks and snippets live, and the rules that keep merchants safe:
  use `{% render %}`, put text in locales, and never edit `settings_data.json` or
  push to the live theme.
- **`.mcp.json`**: [Shopify's Dev MCP server](https://shopify.dev/docs/apps/build/devmcp)
  (`@shopify/dev-mcp`), which gives Claude Shopify's docs and schemas. It runs
  locally and needs no API key.
- **`.claude/settings.json`**: `shopify theme check` runs freely, `push` and `pull`
  ask first, and `publish` and `delete` are blocked.

```bash
cp examples/shopify-theme/CLAUDE.md examples/shopify-theme/.mcp.json path/to/your-theme/
mkdir -p path/to/your-theme/.claude
cp examples/shopify-theme/.claude/settings.json path/to/your-theme/.claude/
```

## Learn more

- [Memory and CLAUDE.md](https://code.claude.com/docs/en/memory)
- [MCP in Claude Code](https://code.claude.com/docs/en/mcp)
- [Settings](https://code.claude.com/docs/en/settings) and [permissions](https://code.claude.com/docs/en/permissions)

## License

MIT. See [LICENSE](LICENSE).

---

Want this set up for your team? <CONTACT_LINK>
