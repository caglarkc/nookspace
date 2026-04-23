# Security Model

## Security Principles

NookSpace is designed around least privilege and scoped execution:

- Restrict file access to authorized workspace paths
- Validate command execution context before running tools
- Keep renderer and system access separated through preload bridges

## File Access Boundaries

- File operations are validated in tool executor layers
- Paths are normalized and checked against allowed mount roots
- Unauthorized paths are rejected

## Command Execution Guardrails

- Shell command execution is sandbox-aware
- Working directory and extracted paths are validated
- Invalid or out-of-scope commands are blocked

## Isolation Modes

Depending on platform and setup:

- Windows: WSL-based isolated execution path
- macOS: Lima-based isolated execution path
- Fallback: constrained local execution with path guards

## User Responsibility

Even with safeguards, AI actions can be destructive if approved blindly.

- Review prompts and tool calls before approving risky actions
- Use dedicated workspaces for sensitive tasks
- Keep backups of critical files
