# Red/Green Testing

When fixing a bug or addressing an issue, always follow the red/green testing pattern:

1. **Red**: First, write or run a test that demonstrates the failure. Show that the test fails. This proves you understand the problem and have a reliable way to detect it.

2. **Green**: Then make the fix. Run the same test again to show it passes.

This applies to:
- Bug fixes: Write a test case that reproduces the bug before fixing it
- CI failures: Reproduce the failure locally before attempting a fix
- Refactors: Run existing tests to establish a baseline before changing code

If you cannot write a failing test first, explain why (e.g., the issue is non-deterministic, requires infrastructure not available locally, etc.).

## Why This Matters

Without a red test first:
- You might fix the wrong thing
- You can't prove your fix actually addresses the issue
- Reviewers can't verify the fix is correct
- The same bug can silently regress later
