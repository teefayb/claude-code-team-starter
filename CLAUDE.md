<!--
  CLAUDE.md template. Claude Code loads this file at the start of every session.

  How to use it:
  - Replace every <PLACEHOLDER> and delete the lines that don't apply.
  - Keep it short. The official guidance is under 200 lines per file;
    every line costs context in every session.
  - Write rules Claude can check: "Run `npm test` before committing",
    not "Test your changes".
  - Only write what Claude can't work out from the code itself.
  - These HTML comments are stripped before the file reaches Claude,
    so leave notes for your team here at no context cost.
-->

# <PROJECT_NAME>

<!-- One or two sentences: what this project is and who uses it. -->
<ONE_LINE_DESCRIPTION>

## Stack

<!-- Only what matters when writing code: language, framework, key versions. -->
- <LANGUAGE / FRAMEWORK + VERSION>
- <PACKAGE_MANAGER> (don't switch to another one)

## Commands

<!-- Exact commands. Claude runs these as written. -->
- Install: `<npm ci>`
- Dev server: `<npm run dev>`
- Test: `<npm test>`
- Lint and format: `<npm run lint>`
- Build: `<npm run build>`

## Project layout

<!-- Where things live, if it isn't obvious from the folder names. -->
- `<src/>`: <what lives here>
- `<tests/>`: <what lives here>

## Conventions

<!-- Rules a reviewer would flag in a PR. -->
- <Use 2-space indentation and named exports.>
- <New code gets a test next to it: `foo.ts` -> `foo.test.ts`.>
- <Commit messages: imperative mood, under 72 characters.>

## Do

- Run the test and lint commands above before saying a task is done.
- Keep changes small and focused on the task you were given.
- Ask before adding a new dependency.

## Don't

- Don't edit generated files in `<dist/>` by hand.
- Don't commit secrets. API keys live in environment variables, never in files.
- Don't push, deploy or publish unless you were asked to.

<!--
  Personal notes that shouldn't be shared? Put them in CLAUDE.local.md
  (already in .gitignore). Long sections that only matter for part of the
  codebase can move to .claude/rules/ files instead.
-->
