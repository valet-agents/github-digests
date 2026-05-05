# End-of-Day Shipped Digest

The cron channel fires once on its schedule (5:30pm Pacific,
Monday–Friday). There is no payload to parse — your job is to run
the EOD digest workflow and post the result to Slack.

## Steps

1. Follow the **EOD Digest Workflow** in SOUL.md (Phases 1–3):
   read the `REPOS` env var, pull today's merged PRs / notable
   issues / failed deploys / releases for each repo via
   `github-mcp`, classify customer-visible vs internal, and
   compose the digest message.
2. Resolve target channels per the SOUL **Where to post** section:
   list every channel the bot is a member of and post once to each.
   If the bot is in zero channels, DM the workspace install user
   instead with the digest and a one-line invite hint.
3. Post exactly once per resolved destination. Do not retry on
   failure — log the error in your session and continue with the
   remaining destinations. The next cron fire is the recovery.
4. Do not send any follow-ups, reactions, or thread replies after
   the initial post. Your turn ends after the posts complete.

## Skip conditions

- **`REPOS` env var unset or empty**: post a single line to the
  resolved destinations: *"No repos configured — set the `REPOS`
  env var on the agent (e.g. `REPOS=owner/repo`)."* If the bot is
  in zero channels, DM the workspace install user with the same
  hint instead. Then stop.
- **No merges today across any configured repo**: post a single
  one-liner — `Nothing shipped today.` — and stop. (Still post it,
  so the team knows the agent ran.)
- **`github-mcp` is unauthenticated or the token has no access to
  any repo in `REPOS`**: stay silent on the cron fire. The next
  human @mention will surface the auth error.
