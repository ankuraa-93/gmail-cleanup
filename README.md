# Gmail Inbox Cleanup — Claude Code Skill

A Claude Code skill that guides you through a complete Gmail inbox cleanup using the [Google Workspace CLI](https://github.com/nicholasgasior/gws). It audits your inbox, classifies senders, bulk-trashes promotions, auto-labels status updates, and unsubscribes you from unwanted senders — all through natural language conversation.

## What it does

1. **Setup** — Checks prerequisites, walks you through OAuth setup, captures baseline inbox stats
2. **Inbox Audit** — Samples emails per Gmail category, builds a sender leaderboard, identifies cleanup tiers
3. **Classification Scan** — Scans last 30 days of inbox, classifies every sender as RELEVANT / STATUS UPDATE / PROMO / SPAM, exports to an editable file for your review
4. **Cleanup Execution** — Trashes promo/spam emails, creates a "Status Updates" label for transactional emails, sets up Gmail filters for future automation
5. **Promo Sweep** — Second pass to catch promo senders that slipped through initial cleanup
6. **Unsubscribe** — Scans trashed emails for unsubscribe links, auto-unsubscribes via RFC 8058 one-click POST, creates a manual list for the rest
7. **Final Summary** — Before/after stats, list of all actions taken, files created

## Safety

- **Trash only, never delete** — Gmail keeps trashed emails for 30 days, giving you time to recover anything
- **Header-only analysis** — Never reads email bodies, eliminating prompt injection risk
- **User approval required** — Exports classifications to a file for your review before taking any action
- **Personal emails untouched** — Never touches the Primary/Personal category unless you explicitly ask

## Prerequisites

- **Node.js** (v18+)
- **Google Cloud project** with Gmail API enabled
- **OAuth 2.0 credentials** (Desktop app type) with these scopes:
  - `https://www.googleapis.com/auth/gmail.readonly`
  - `https://www.googleapis.com/auth/gmail.modify`
  - `https://www.googleapis.com/auth/gmail.settings.basic` (for filter creation)
- **Claude Code** installed ([claude.ai/code](https://claude.ai/code))

The skill will walk you through the Google Cloud setup if you haven't done it before.

## Installation

### Option 1: Project-level (recommended)

Copy the skill file into your project's `.claude/commands/` directory:

```bash
mkdir -p .claude/commands
curl -o .claude/commands/gmail-cleanup.md https://raw.githubusercontent.com/ankuraa93/gmail-cleanup-skill/main/gmail-cleanup.md
```

### Option 2: Global (available in all projects)

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/gmail-cleanup.md https://raw.githubusercontent.com/ankuraa93/gmail-cleanup-skill/main/gmail-cleanup.md
```

## Usage

Open Claude Code and run:

```
/gmail-cleanup
```

Claude will guide you through each phase. You can stop at any point — progress is saved to `progress.md` and picks up where you left off in the next session.

## Files created during cleanup

| File | Purpose |
|------|---------|
| `progress.md` | Full progress tracker — stats, label IDs, filter IDs, next steps |
| `sender_classification.md` | Every sender classified as RELEVANT or PROMO (editable) |
| `full_unsubscribe_list.md` | All senders with unsubscribe links from trash scan |
| `manual_unsubscribe.md` | Senders that need manual unsubscribe (clickable URLs) |

## Tips

- Run `/clear` between phases to free up context — `progress.md` preserves everything you need
- The skill batches API calls to stay within rate limits; large inboxes may take 20-30 minutes for full scans
- You can edit classification files to move senders between RELEVANT and PROMO before cleanup executes

## License

MIT
