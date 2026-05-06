---
name: generate-github-mcp-server-pr-description
description: Fills out a PR description for the github/github-mcp-server repository. Use this when asked to write, draft, or fill out a PR description for the GitHub MCP Server repo.
---

Use this skill when asked to write or fill out a PR description for the `github/github-mcp-server` repository.

## Steps

1. **Understand the change** — read the diff or ask the user what was changed if not clear. Use `git diff main...HEAD` or `git log main..HEAD --oneline` to summarise commits.

2. **Ask the user about related issues** — before filling the template, ask:
   > "Are there any GitHub issues related to this PR I should look up for context? If so, provide the issue number(s) and I'll fetch them."
   
   If the user provides issue numbers, use `gh issue view <number> --repo github/github-mcp-server` to fetch each one and incorporate the context (title, body, linked discussion, motivation) into the **Why** and **Summary** sections.

3. **Fill out the PR template** below. Prefer short, concrete answers. Only check boxes that apply.

4. **Output the completed description** as a markdown code block the user can paste directly.

---

## PR Template (github/github-mcp-server)

```markdown
## Summary
<!-- In 1–2 sentences: what does this PR do? -->

## Why
<!-- Why is this change needed? Link issues or discussions. -->
Fixes #

## What changed
<!-- Bullet list of concrete changes. -->
- 
- 

## MCP impact
<!-- Select one or more. If selected, add 1–2 sentences. -->
- [ ] No tool or API changes
- [ ] Tool schema or behavior changed
- [ ] New tool added

## Prompts tested (tool changes only)
<!-- If you changed or added tools, list example prompts you tested. -->
<!-- Include prompts that trigger the tool and describe the use case. -->
<!-- Example: "List all open issues in the repo assigned to me" -->
- 

## Security / limits
<!-- Select if relevant. Add a short note if checked. -->
- [ ] No security or limits impact
- [ ] Auth / permissions considered
- [ ] Data exposure, filtering, or token/size limits considered

## Tool renaming
- [ ] I am renaming tools as part of this PR (e.g. a part of a consolidation effort)
   - [ ] I have added the new tool aliases in `deprecated_tool_aliases.go` 
- [ ] I am not renaming tools as part of this PR

## Lint & tests
<!-- Check what you ran. If not run, explain briefly. -->
- [ ] Linted locally with `./script/lint`
- [ ] Tested locally with `./script/test`

## Docs
- [ ] Not needed
- [ ] Updated (README / docs / examples)
```

---

## Guidance per section

### Summary
One or two sentences. State *what* the PR does, not *how*. E.g. "Adds `add_discussion_comment` and `delete_discussion_comment` tools to support write operations on GitHub Discussions."

### Why
Link to the originating issue or discussion if one exists. If this is a self-contained improvement, explain the user value. E.g. "Enables AI agents to participate in GitHub Discussions, not just read them."

### What changed
Concrete bullet points per file/area touched:
- New tool `add_discussion_comment` in `pkg/github/discussions.go`
- New tool `delete_discussion_comment` in `pkg/github/discussions.go`
- Updated toolsnaps in `pkg/github/__toolsnaps__/`
- Updated `README.md` via `script/generate-docs`

### MCP impact
- Check **"New tool added"** and name the tool(s).
- Check **"Tool schema or behavior changed"** if you modified an existing tool's parameters or return shape.
- If toolsnaps were regenerated, mention it here.

### Prompts tested
List 2–4 prompts a user could give to an AI agent that would exercise the new/changed tools. Be concrete:
- "Add a comment to discussion #42 in the github-mcp-server repo saying 'Thanks for the update!'"
- "Delete my comment on discussion #42"

### Security / limits
- For write tools: always check **"Auth / permissions considered"** and note that the token requires the appropriate GitHub scope (e.g. `write:discussion`).
- For data-returning tools: check **"Data exposure, filtering, or token/size limits considered"** if responses could be large.

### Tool renaming
Almost always check **"I am not renaming tools"** unless this is an explicit rename/consolidation PR.

### Lint & tests
Check both boxes once you've run `./script/lint` and `./script/test` locally. If toolsnaps changed, note: "Toolsnaps updated with `UPDATE_TOOLSNAPS=true go test ./...`".

### Docs
- Check **"Updated"** if you ran `script/generate-docs` and committed the README changes.
- Check **"Not needed"** only if no tool definitions changed.
