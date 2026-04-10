# Session Analysis

At the end of every task (or when the user says "done", "that's it", "thanks", etc.), offer:

> Would you like me to analyze this session for improvement opportunities? I can check the session log for errors, retries, and wasted tokens.

If the user accepts, perform the analysis below.

## Finding the Session JSONL

Claude Code stores session logs as JSONL files. To find the current session's log:

```bash
# Find the most recently modified JSONL in the project's .claude directory
# The project path is encoded with dashes replacing slashes
PROJECT_DIR=$(pwd | sed 's|/|-|g; s|^-||')
find ~/.claude/projects/ -maxdepth 2 -name "*.jsonl" -not -path "*/subagents/*" \
  -newer /tmp/.session_start 2>/dev/null | head -5

# If that doesn't work, find the most recently written JSONL across all projects
find ~/.claude/projects/ -maxdepth 2 -name "*.jsonl" -not -path "*/subagents/*" \
  -printf '%T@ %p\n' 2>/dev/null | sort -rn | head -5
```

## What to Look For

Grep the JSONL for patterns that indicate wasted work:

```bash
# Errors and failures
grep -i '"error"' <session.jsonl> | head -20
grep -i 'permission denied\|command not found\|ENOENT\|No such file' <session.jsonl> | head -10

# Tool call failures (retried commands)
grep '"tool_use"' <session.jsonl> | grep -i 'error\|failed\|denied' | head -10

# Count retries on similar commands
grep '"tool_use"' <session.jsonl> | jq -r '.content[]?.name // empty' 2>/dev/null | sort | uniq -c | sort -rn | head -10
```

## Output Format

After analysis, produce:

1. **Errors found**: List each distinct error with count and what caused it
2. **Token waste**: Estimate how many retries/detours happened and what could have prevented them
3. **CLAUDE.md recommendations**: Specific lines to add to CLAUDE.md files that would have prevented these errors
4. **Missing context**: Information that wasn't in any CLAUDE.md but should have been

Keep the output actionable — each recommendation should be a concrete sentence that could be copy-pasted into a CLAUDE.md file.
