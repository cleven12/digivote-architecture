# Diagrams

All architecture diagrams for MWECAU DigiVote.

Use https://mermaid.live to view or edit any `.mmd` file.

These diagrams are maintained as standalone files so they can be easily included in other documents or presentations.

## Available Diagrams

| File                        | Description |
|-----------------------------|-------------|
| `system-overview.mmd`       | High-level system context |
| `full-stack.mmd`            | Layered architecture view |
| `cache-layer.mmd`           | Redis usage across the system |
| `celery-architecture.mmd`   | Task queues and workers |
| `deployment.mmd`            | Current production deployment |
| `request-lifecycle.mmd`     | How a request flows end-to-end |
| `voter-journey.mmd`         | End-to-end voter experience |
| `secure-voting.mmd`         | Voting sequence with atomic token usage |
| `election-lifecycle.mmd`    | State machine for elections |
| `roles.mmd`                 | User roles |
| `data-model.mmd`            | Core entities and relationships |

To embed in Markdown:

```mermaid
flowchart TD
    A --> B
```
