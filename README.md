# github-copilot-cli-settings

Portable dev environment for GitHub Copilot CLI in Enterprise work

> 以下作成中（Chapter 04 まで完了）

# メモ

## Modes and Commands (Chapter 01)

| Mode         | Dining Analogy              | When to Use                                                                    |
| ------------ | --------------------------- | ------------------------------------------------------------------------------ |
| Plan         | GPS route to the restaurant | Complex tasks - map out the route, review stops, agree on the plan, then drive |
| Interactive  | Talking to the waiter       | Exploration and iteration - ask questions, customize, get real-time feedback   |
| Programmatic | Drive-through ordering      | Quick, specific tasks - stay in your environment, get a result fast            |

Once you're comfortable, try:

- Programmatic mode (`copilot --allow-all -p "<your prompt>"`) for quick, one-off questions
- Plan mode (`/plan`) when you need to plan things out in more detail before coding

## Context (Chapter 02)

### Basic `@` Patterns

| Pattern               | What It Does                       | Example Use                                                                        |
| --------------------- | ---------------------------------- | ---------------------------------------------------------------------------------- |
| `@file.py`            | Reference a single file            | `Review @samples/book-app-project/books.py`                                        |
| `@folder/`            | Reference all files in a directory | `Review @samples/book-app-project/`                                                |
| `@file1.py @file2.py` | Reference multiple files           | `Compare @samples/book-app-project/book_app.py @samples/book-app-project/books.py` |

> 💡 Permission flags (`--add-dir`, `--allow-all`) control multi-directory access.

## Session Management (Chapter 02)

### Resume the Most Recent Session

```bash
# Continue where you left off
copilot --continue
```

### Resume a Specific Session

```bash
# Pick from a list of sessions interactively
copilot --resume

# -r is a shorthand for --resume (saves some typing!)
copilot -r

# Or resume a specific session by ID
copilot --resume=abc123

# Or resume by the name you gave the session
copilot --resume="my book app review"
```

### Organize Your Sessions

Give sessions meaningful names so you can find them later. You can name a session when you start it, or rename it at any time while inside the session:

```bash
# Name a session right when you start it
copilot --name book-app-review

# Or rename the current session from inside
copilot

> /rename book-app-review
# Session renamed for easier identification
```

Once a session is named, you can resume it directly by name without browsing through a list:

```bash
copilot --resume=book-app-review
```

To clean up sessions you no longer need, use `/session delete` from inside a session:

```bash
copilot

> /session delete            # Deletes the current session
> /session delete abc123     # Deletes a specific session by ID
> /session delete-all        # Deletes all sessions (use with care!)
```

### Persistent Memory Across Sessions

Sessions save your conversation history, but <mark style="background: #FF5582A6;">**memory** goes one step further and lets Copilot CLI remember preferences and facts _across all sessions_, not just within a single one.</mark>

```bash
copilot

> /memory show
# Shows what Copilot CLI currently remembers about you and your project

> /memory on
# Enables memory (on by default if your account supports it)

> /memory off
# Disables memory (useful if you prefer a fresh slate each time)
```

### Check and Manage Context

As you add files and conversation, Copilot CLI's context window fills up. Several commands are available to help you stay in control:

```bash
copilot

> /context
Context usage: 62k/200k tokens (31%)

> /clear
# Abandons the current session (no history saved) and starts a fresh conversation

> /new
# Ends the current session (saving it to history for search/resume) and starts a fresh conversation

> /rewind
# Opens a timeline picker allowing you to roll back to an earlier point in your conversation
```

> 💡 **When to use `/clear` or `/new`**: If you've been reviewing books.py and want to switch to discussing utils.py, run /new first (or /clear if you don't need the session history). Otherwise stale context from the old topic may confuse responses.

> 💡 **Made a mistake or want to try a different approach?** Use `/rewind` (or press Esc twice) to open a **timeline picker** that lets you roll back to any earlier point in your conversation, not just the most recent one. This is useful when you went down the wrong path and want to backtrack without starting over entirely.

### Additional `@` Patterns

For power users, Copilot CLI supports wildcard patterns and image references:

| Pattern         | What It Does                                     |
| --------------- | ------------------------------------------------ |
| `@folder/*.py`  | All .py files in folder                          |
| `@**/test_*.py` | Recursive wildcard: find all test files anywhere |
| `@image.png`    | Image file for UI review                         |

```bash
copilot

> Find all TODO comments in @samples/book-app-project/**/*.py
```

### View Session Info

```bash
copilot

> /session
# Shows current session details and workspace summary

> /usage
# Shows session metrics and statistics
```

### Share Your Session

```bash
copilot

> /share file ./my-session.md
# Exports session as a markdown file

> /share gist
# Creates a GitHub gist with the session

> /share html
# Exports session as a self-contained interactive HTML file
# Useful for sharing polished session reports with teammates or saving for reference
```

### The `/compact` Command

When your context is getting full but you don't want to lose the conversation, `/compact` summarizes your history to free up tokens:

```bash
copilot

> /compact
# Summarizes conversation history, freeing up context space
# Your key findings and decisions are preserved
```

You can also give `/compact` optional focus instructions to shape what gets prioritized in the summary:

```bash
copilot

> /compact focus on the list of bugs we found and decisions made
# Summarizes history, keeping bug list and decisions prominent
```

> 💡 **When to use focus instructions**: If your conversation covered many topics, focus instructions help `/compact` retain the parts most relevant to your next steps so you don't lose the thread.

### Context Efficiency Tips

| Situation            | Action                    | Why                              |
| -------------------- | ------------------------- | -------------------------------- |
| Starting new topic   | `/clear`                  | Removes irrelevant context       |
| Went down wrong path | `/rewind`                 | Roll back to any earlier point   |
| Long conversation    | `/compact`                | Summarizes history, frees tokens |
| Need specific file   | `@file.py` not `@folder/` | Loads only what you need         |
| Hitting limits       | `/new` or `/clear`        | Fresh context                    |
| Multiple topics      | Use `/rename` per topic   | Easy to resume right session     |

## Development Workflow (Chatper 03)

> 一般的な流れに沿って開発フローを体験
> code-review ->
> refactoring ->
> Debugging ->
> Test Generation ->
> Git Integration (Fin)

### Using the `/review` Command

The `/review` command invokes the built-in **code-review agent**, which is optimized for analyzing staged and unstaged changes with high signal-to-noise output. Use a slash command to trigger a specialized built-in agent instead of writing a free-form prompt.

```bash
copilot

> /review
# Invokes the code-review agent on staged/unstaged changes
# Provides focused, actionable feedback

> /review Check for security issues in authentication
# Run review with specific focus area
```

### Using `/pr` in Interactive Mode for the Current Branch

If you're working with a branch in Copilot CLI's interactive mode, you can use the `/pr` command to work with pull requests. Use `/pr` to view a PR, create a new PR, fix an existing PR, or let Copilot CLI auto-decide based on the branch state.

```bash
copilot

> /pr [view|create|fix|auto]
```

### Review Before Push

Use `git diff main..HEAD` inside a `-p` prompt for a quick pre-push sanity check across all branch changes.

```bash
# Last check before pushing
copilot -p "Review these changes for issues before I push:
$(git diff main..HEAD)"
```

### Using `/delegate` for Background Tasks

The `/delegate` command hands off work to the GitHub Copilot cloud agent. Use the `/delegate` slash command (or the `&` shortcut) to offload a well-defined task to a background agent.

```bash
copilot

> /delegate Add input validation to the login form

# Or use the & prefix shortcut:
> & Fix the typo in the README header

# Copilot CLI:
# 1. Commits your changes to a new branch
# 2. Opens a draft pull request
# 3. Works in the background on GitHub
# 4. Requests your review when done
```

This is great for well-defined tasks you want completed while you focus on other work.

### Using `/diff` to Review Session Changes

The `/diff` command shows all changes made during your current session. Use this slash command to see a visual diff of everything Copilot CLI has modified before you commit. It also works in folders that aren't git repositories.

```bash
copilot

# After making some changes...
> /diff

# Shows a visual diff of all files modified in this session
# Great for reviewing before committing
```

### Branching Your Session with `/branch` or `/fork`

Sometimes you want to explore two different approaches to a problem without losing your original conversation. The `/branch` command (also available as `/fork`) creates a copy of your current session so you can try a different direction and then compare results.

```bash
copilot

> Fix the find_by_author function to support partial matches

# You want to try a different approach — branch first!
> /branch

# Now you're in a new session copy. Try your alternative approach:
> Fix find_by_author using a different regex-based strategy

# If you don't like the result, switch back to your original session using /session
```

> 💡 **`/branch` and `/fork` are the same**: Both commands do identical things. `/branch` was added as a more intuitive name. Use whichever makes more sense to you.

> 💡 **When to branch**: Branching is great when you're unsure which approach is better and want to keep both options open.

## `*.agents.md` for custom instructions (Chapter 04)

- <mark style="background: #FF5582A6;">他社のエージェントツールと異なる、GitHub Copilot の独自機能</mark>

### Add your agents

Agent files are markdown files with a `*.agent.md` extension. They have two parts: YAML frontmatter (metadata) and markdown instructions.

> 💡 **New to YAML frontmatter?** It's a small block of settings at the top of the file, surrounded by `---` markers. YAML is just `key: value` pairs. The rest of the file is regular markdown.

Here's a minimal agent:

```markdown
---
name: my-reviewer
description: Code reviewer focused on bugs and security issues
---

# Code Reviewer

You are a code reviewer focused on finding bugs and security issues.

When reviewing code, always check for:

- SQL injection vulnerabilities
- Missing error handling
- Hardcoded secrets
```

> 💡 **Required vs Optional**: The `description` field is required. Other fields like `name`, `tools`, and `model` are optional.

#### Where to put agent files

| Location                       | Scope                 | Best For                                    |
| ------------------------------ | --------------------- | ------------------------------------------- |
| `.github/agents/*.agent.md`    | Project-specific      | Team-shared agents with project conventions |
| `~/.copilot/agents/*.agent.md` | Global (all projects) | Personal agents you use everywhere          |

#### `AGENTS.md` との違い

- 他のコーディングエージェント製品で言うところのプロファイル切り替えに近いイメージ
- 各エージェントは独自のコンテキストウィンドウを持つため、メインのコンテキストウィンドウ節約の用途でも使える
- 推奨命名規則： lowercase with hyphens

| 仕組み       | 役割                         | 適用イメージ                       |
| ------------ | ---------------------------- | ---------------------------------- |
| `AGENTS.md`  | プロジェクトで守る共通ルール | 常時参照される「AI向けREADME」     |
| `*.agent.md` | 名前付きの専門エージェント   | 必要な場面で選択・委譲する専門担当 |

### Two ways to use custom agents

#### Interactive mode

Inside interactive mode, list agents using `/agent` and select the agent to start working with.
Select an agent to continue your conversation with.

```bash
copilot
> /agent
```

To change to a different agent, or to return to default mode, use the `/agent` command again.

#### Programmatic mode

Launch straight into a new session with an agent.

```bash
copilot --agent python-reviewer
> Review @samples/book-app-project/books.py
```

> 💡 **Switching agents**: You can switch to a different agent at any time by using `/agent` or `--agent` again. To return to the standard Copilot CLI experience, use `/agent` and select **no agent**.

> 💡 **Agent mode is session-scoped**: The agent you select applies only to the current session. When you start a new session with `/new`, `/clear`, or by opening a fresh terminal, Copilot returns to its default mode — your agent selection does not carry over automatically. This means each session starts with a clean slate, which is a good habit to keep your work focused.

### Instruction File Formats

| File                                           | Scope                  | Notes                                                                    |
| ---------------------------------------------- | ---------------------- | ------------------------------------------------------------------------ |
| `AGENTS.md`                                    | Project root or nested | **Cross-platform standard** - works with Copilot and other AI assistants |
| `.github/copilot-instructions.md`              | Project                | GitHub Copilot specific                                                  |
| `.github/instructions/*.instructions.md`       | Project                | Granular, topic-specific instructions                                    |
| `~/.copilot/instructions/**/*.instructions.md` | User (all projects)    | Personal instructions that apply everywhere, across all your repos       |
| `CLAUDE.md`, `GEMINI.md`                       | Project root           | Supported for compatibility                                              |

### Scoping Instructions with `applyTo`

By default, an instruction file applies to every conversation. To limit it to specific file types, add an `applyTo` field in YAML frontmatter (the block between `---` markers at the very top of the file):

```markdown
---
applyTo: "**/*.py"
---

# Python Standards

Always follow PEP 8 style conventions.
Use type hints in all function signatures.
```

With `applyTo: "**/*.py"`, Copilot only loads that instruction file when you are working with Python files. Instructions for Python style never clutter a conversation about, say, a Dockerfile or a SQL query.

Here are some common patterns:

| `applyTo` value   | When it applies                   |
| ----------------- | --------------------------------- |
| `"**/*.py"`       | Any Python file                   |
| `"**/*.{ts,tsx}"` | TypeScript and TSX files          |
| `"tests/**"`      | Any file inside a `tests/` folder |
| (no frontmatter)  | Every conversation — the default  |

> 💡 **Tip**: Wrap the glob pattern in quotes (e.g., `"**/*.py"`) to ensure it is interpreted correctly across all operating systems and shells.

### Importing Other Files with `@`

You can reference another file inside `AGENTS.md` or any instruction file using `@filepath` syntax. Copilot expands the reference and includes that file's content automatically, so you can keep your main file short while storing the details elsewhere:

```markdown
<!-- AGENTS.md -->

# Project Instructions

@.github/instructions/python-standards.instructions.md
@.github/instructions/test-standards.instructions.md
```

This is handy when your instructions grow large. Split them into focused files and `@`-import them from a single `AGENTS.md`. The same syntax works inside `.github/copilot-instructions.md` and other instruction files too.

> 💡 **Tip**: Use `@`-imports to share a common base file across multiple instruction files. For example, you could have a `@.github/instructions/shared-rules.md` that every other instruction file pulls in.

**Finding community instruction files**: Browse [github/awesome-copilot](https://github.com/github/awesome-copilot) for pre-made instruction files covering .NET, Angular, Azure, Python, Docker, and many more technologies.

### Disabling Custom Instructions

If you need Copilot to ignore all project-specific configurations (useful for debugging or comparing behavior):

```bash
copilot --no-custom-instructions
```

### Sample with `tools` property

Here's a more comprehensive agent that uses the `tools` property. Create `~/.copilot/agents/python-reviewer.agent.md`:

```markdown
---
name: python-reviewer
description: Python code quality specialist for reviewing Python projects
tools: ["read", "edit", "search", "execute"]
---

# Python Code Reviewer

You are a Python specialist focused on code quality and best practices.

**Your focus areas:**

- Code quality (PEP 8, type hints, docstrings)
- Performance optimization (list comprehensions, generators)
- Error handling (proper exception handling)
- Maintainability (DRY principles, clear naming)

**Code style requirements:**

- Use Python 3.10+ features (dataclasses, type hints, pattern matching)
- Follow PEP 8 naming conventions
- Use context managers for file I/O
- All functions must have type hints and docstrings

**When reviewing code, always check:**

- Missing type hints on function signatures
- Mutable default arguments
- Proper error handling (no bare except)
- Input validation completeness
```

#### YAML Properties

| Property      | Required | Description                                                                 |
| ------------- | -------- | --------------------------------------------------------------------------- |
| `name`        | No       | Display name (defaults to filename)                                         |
| `description` | **Yes**  | What the agent does - helps Copilot understand when to suggest it           |
| `tools`       | No       | List of allowed tools (omit = all tools available). See tool aliases below. |
| `target`      | No       | Limit to `vscode` or `github-copilot` only                                  |

#### Tool Aliases

Use these names in the `tools` list:

- `read` - Read file contents
- `edit` - Edit files
- `search` - Search files (grep/glob)
- `execute` - Run shell commands (also: `shell`, `Bash`)
- `agent` - Invoke other custom agents

> 📖 **Official docs**: [Custom agents configuration](https://docs.github.com/copilot/reference/custom-agents-configuration)
>
> ⚠️ **VS Code Only**: The `model` property (for selecting AI models) works in VS Code but is not supported in GitHub Copilot CLI. You can safely include it for cross-platform agent files. GitHub Copilot CLI will ignore it.

### Common Mistakes

| Mistake                                    | What Happens                              | Fix                                                                     |
| ------------------------------------------ | ----------------------------------------- | ----------------------------------------------------------------------- |
| Missing `description` in agent frontmatter | Agent won't load or won't be discoverable | Always include `description:` in YAML frontmatter                       |
| Wrong file location for agents             | Agent not found when you try to use it    | Place in `~/.copilot/agents/` (personal) or `.github/agents/` (project) |
| Using `.md` instead of `.agent.md`         | File may not be recognized as an agent    | Name files like `python-reviewer.agent.md`                              |
| Overly long agent prompts                  | May hit the 30,000 character limit        | Keep agent definitions focused; use skills for detailed instructions    |

---

# 元ネタ

- [n-ogura-ep/copilot-cli-for-beginners](https://github.com/n-ogura-ep/copilot-cli-for-beginners)

> 参考

- [GitHub Copilot CLIを"タダ"で体系的に学べるコースをやってみた](https://zenn.dev/microsoft/articles/9fb85cdca2fb84)
