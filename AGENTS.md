This folder contains the source for a Skilled Agent originally built for the Valet runtime. Changes should follow the Skilled Agent open standard.

## Setup

### Connectors

- **github-mcp**: The GitHub MCP server, authenticated with a Personal Access Token. The agent uses it to read merged PRs, issues, workflow runs, and releases for the configured repos, and (when explicitly asked in Slack) to open issues, comment, or label. Add it from the catalog at the org level so other GitHub-powered agents can share the same token slot.

### Channels

- **slack** (slack): The agent's per-agent Slack bot. Listens for @mentions and replies in-thread, and posts the EOD digest to whichever channels the bot has been invited to. Slack writes use the auto-injected outbound Slack connector.
- **cron** (cron): Fires the end-of-day shipped digest at 5:30pm Pacific, Monday through Friday (`30 17 * * 1-5`, `America/Los_Angeles`). Declared inline in `valet.yaml`, so it's created automatically by the dashboard setup flow.

### Secrets

- **GITHUB_PERSONAL_ACCESS_TOKEN**: a Personal Access Token from https://github.com/settings/tokens. A **fine-grained PAT** scoped to just the repos you want summarized is preferred — grant read-only access to Pull Requests, Issues, and Contents. A classic PAT with the `repo` scope also works but is broader than this agent needs. The token is stored once on the `github-mcp` connector and never echoed in Slack.

### External Setup

1. After deploy, set the `GITHUB_PERSONAL_ACCESS_TOKEN` secret on the `github-mcp` connector in the dashboard. Use a fine-grained PAT scoped to the repos you'll list in `REPOS`.
2. Set the `REPOS` env var on the agent — comma-separated `owner/repo` (e.g. `REPOS=valet-dev/valet,valet-dev/site`). Without it, the agent will post a one-line "no repos configured" hint and stop.
3. Invite the agent's Slack bot to whichever channel(s) you want the EOD digest in. The agent posts the digest to every channel it's a member of — invite it to one focused channel, or several. If the bot has not been invited anywhere, the digest is sent as a DM to the workspace install user with a one-line nudge to invite it somewhere.
4. Invite the bot to any additional channels where teammates should be able to @mention it for ad-hoc GitHub questions (e.g. an engineering channel for "what shipped today?" or "is the deploy green?" follow-ups).
5. The first cron fire is the next 5:30pm Pacific weekday after deploy. To smoke-test sooner, @mention the bot in Slack with a question like *"what shipped today?"* — that exercises the Slack + GitHub path without waiting for the cron.

## Customizing

- **Change the schedule**: edit the `cron` and `timezone` on the `cron` channel in `valet.yaml`, then redeploy. The default `30 17 * * 1-5` `America/Los_Angeles` lands the digest at 5:30pm Pacific on weekdays.
- **Change the repo list**: edit the `REPOS` env var on the agent — comma-separated `owner/repo`. Add or remove repos without touching the SOUL.
- **Tune the customer-visible heuristic**: the SOUL classifies a merged PR as customer-visible if it carries labels like `release-note`, `customer-facing`, `feature`, `bug`, `ux`, or has a `feat:`/`fix:` title prefix. Adjust the label list in SOUL.md to match the conventions your team already uses (e.g. `changelog`, `user-impact`).
