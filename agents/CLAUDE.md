# CLAUDE.md — Personal Patterns

## Commit Convention

Semantic commits, always. Format:

```
<type>(<scope>): <description>
```

### Commit Types

| Type       | When to use                                 |
|------------|---------------------------------------------|
| `feat`     | New feature or capability                   |
| `fix`      | Bug fix                                     |
| `refactor` | Code change that doesn't fix or add         |
| `chore`    | Tooling, config, deps                       |
| `docs`     | Documentation only                          |
| `test`     | Adding or fixing tests                      |
| `perf`     | Performance improvement                     |

### Suggested Commit

After every modification, suggest the full commit command:

```bash
git add <files>
git commit -m "<type>(<scope>): <description>"
```

Never omit this. Always at the end of the response.

---

## CLAUDE.md

- Update proactively — never wait to be asked
- If a pattern changes, reflect it here immediately

---

## README

- Keep in sync with any user-visible change
- If a command, flag, or behavior changes — update README in the same response

---

## Git History Rewrite

Use `git commit-tree` when rewriting history is needed. Never suggest interactive rebase as the only option.

---

## Project-Specific (j)

### Scopes

| Scope      | When to use                        |
|------------|------------------------------------|
| `core`     | Traits, pipeline, capabilities     |
| `plugin`   | Any plugin change                  |
| `cli`      | Binary, wiring, entry point        |
| `tui`      | Terminal UI layer                  |
| `files`    | files plugin specifically          |
| `help`     | help plugin specifically           |

### Plugin Table

Maintain this in README:

| Plugin  | Trigger     | Capability           | Status  |
|---------|-------------|----------------------|---------|
| files   | `<query>`   | SearchCapability     | ✅ active |
| help    | `?`         | SearchCapability     | ✅ active |

### Checklist — Adding a New Plugin

- [ ] Create `/plugins/<name>/Cargo.toml`
- [ ] Implement `Plugin` trait
- [ ] Implement required capability trait(s)
- [ ] Register in `cli/main.rs`
- [ ] Add to plugin table in README
- [ ] Update this CLAUDE.md if new patterns emerge

### Rust / Tokio Constraints

- `core` has zero UI dependencies
- Plugins depend only on `core`
- `cli` is as thin as possible
- Async runtime: `tokio` only
- File traversal: `walkdir`
- Search is streaming and cancellable
