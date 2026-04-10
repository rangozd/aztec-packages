# Session Analysis

At the end of every task (or when the user says "done", "that's it", "thanks", etc.), offer:

> Would you like me to analyze this session for improvement opportunities? I can check the session log for errors, retries, and wasted tokens.

If the user accepts, perform the analysis below.

## Finding the Session JSONL

Claude Code stores session logs as JSONL files under `~/.claude/projects/`. The project path is encoded by replacing `/` with `-`. To find the current session's log:

```bash
# Find the most recently written JSONL across all projects (most reliable)
find ~/.claude/projects/ -maxdepth 2 -name "*.jsonl" -not -path "*/subagents/*" \
  -printf '%T@ %p\n' 2>/dev/null | sort -rn | head -5
```

Pick the file matching this project's encoded path (e.g., a project at `/home/user/aztec-packages` becomes `-home-user-aztec-packages`).

## What to Look For

Grep the JSONL for patterns that indicate wasted work:

```bash
SESSION_JSONL="<path from above>"

# Permission blocks and errors (most common waste)
grep -c 'sensitive file' "$SESSION_JSONL"
grep -c 'is_error.*true' "$SESSION_JSONL"

# Specific error types
grep -i 'permission denied\|command not found\|No such file\|ENOENT' "$SESSION_JSONL" | wc -l

# Failed tool calls — look for error results
grep '"is_error":true' "$SESSION_JSONL" | grep -oP '"content":"[^"]{0,200}' | head -10
```

## Output Format

After analysis, produce:

1. **Errors found**: List each distinct error with count and what caused it
2. **Token waste**: Estimate how many retries/detours happened and what could have prevented them
3. **CLAUDE.md recommendations**: Specific lines to add to CLAUDE.md files that would have prevented these errors
4. **Missing context**: Information that wasn't in any CLAUDE.md but should have been

Keep the output actionable — each recommendation should be a concrete sentence that could be copy-pasted into a CLAUDE.md file.
