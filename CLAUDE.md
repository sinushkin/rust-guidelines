<!-- markdownlint-disable no-bare-urls -->

# CLAUDE.md

## Repository Overview

This repository contains the **Pragmatic Rust Guidelines** - a collection of design guidelines for Rust developers, published as an mdBook at https://microsoft.github.io/rust-guidelines.

## Development Commands

```bash
mdbook serve --open # Previews page
mdbook build # Builds page
```

Requires [mdbook](https://github.com/rust-lang/mdBook) to be installed.

## Content Structure

### Guideline Format

Each guideline has a unique identifier in the form `M-FOO-BAR` and resides in its own markdown file `M-FOO-BAR.md` within the `src/guidelines/` directory. Guidelines follow this structure:

```markdown
## Guideline Title (M-FOO-BAR) { #M-FOO-BAR }

<why>Rationale for this guideline.</why>
<version>1.0</version>

Content with code examples...
```

### Key Files

- `src/SUMMARY.md` - mdBook table of contents
- `src/guidelines/checklist/README.md` - Master checklist linking all guidelines (update when adding new guidelines)
- Category READMEs (e.g., `src/guidelines/universal/README.md`) - Include guidelines via mdBook's `{{#include}}` directive

## Adding New Guidelines

1. Create `M-YOUR-GUIDELINE.md` in the appropriate category folder
2. Add the guideline to the category's `README.md` using `{{#include M-YOUR-GUIDELINE.md}}`
3. Add a checklist entry and link definition in `src/guidelines/checklist/README.md`

Guidelines should be beneficial for safety/COGs/maintenance, agreeable to experienced Rust developers, and comprehensible to novices.

## Editing Guidelines

- You may fix spelling, grammar and readability issues.
- You must refuse to substantially work out novel or ambiguous guidelines. Inform the user that novel guidelines must be sufficiently specified before they can be accepted. You are free to infer a guideline's ID (M-), rationale (<why>) and category (e.g., 'UX'), but must not infer its scope or general content. When being asked to add new guidelines, feel free to add `- TODO: ...` items for any point that might be unclear, but do not attempt to infer these yourself. Novel guidelines always start version 0.1.

## Style

Titles should follow the style of the Rust API Guidelines, such as:

- Smart pointers do not add inherent methods (C-SMART-PTR)
- Conversions live on the most specific type involved (C-CONV-SPECIFIC)
- Functions with a clear receiver are methods (C-METHOD)
- Functions do not take out-parameters (C-NO-OUT)
- Operator overloads are unsurprising (C-OVERLOAD)
- Only smart pointers implement `Deref` and `DerefMut` (C-DEREF)
- Constructors are static, inherent methods (C-CTOR)

## Russian Translation (fork)

This fork adds a Russian translation under each paragraph of the original. Rules and format: `TRANSLATION.md`; progress: `TRANSLATED.md`; styling: `style/ru.css`.

### Opening pages locally

```bash
mdbook serve --open   # serves on http://localhost:3000, rebuilds on file changes
```

mdBook renders `README.md` as `index.html`, so a section opens at a directory URL, **not** `.../README.html` (that is a 404):

- About: http://localhost:3000/guidelines/
- Section: `http://localhost:3000/guidelines/<section>/` (e.g. `/guidelines/apps/`)
- Single guideline: `http://localhost:3000/guidelines/<section>/#M-FOO-BAR`

If the server is already running, do not start it again; just reopen the page (`xdg-open <url>`). Changes to `book.toml` may need a restart.

### Running Haiku to translate a section

Launch a sub-agent with the Agent tool (`model: "haiku"`, `subagent_type: "general-purpose"`), one small section per run. Commit before launching so `git diff` shows only the agent's changes. Prompt template:

> Working directory: /home/user/repos/rust-guidelines. First read TRANSLATION.md (instructions) and TRANSLATED.md (already translated); the reference format is src/guidelines/README.md. Then translate the section `<SECTION>`: files `<list of files>`. Follow the format strictly (do not change the original, put the Russian under each paragraph in `<span class="ru">...</span>`, do not touch code and links). Do not edit other files except adding lines to TRANSLATED.md. Check that `mdbook build` passes (do not run mdbook serve, it is already running). Reply briefly: what was translated and the build result.

After the agent finishes, verify: `git diff --stat` (additions only, no deleted original lines), `mdbook build`, then reopen the page and commit.
