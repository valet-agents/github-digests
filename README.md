# GitHub Digests

End of each day it summarizes merges, deploys, and customer-visible changes — what shipped, what broke, what users said.

## Prerequisites
- A [GitHub](https://github.com) Personal Access Token with `repo` scope (or a fine-grained PAT with read-only access to Pull Requests, Issues, and Contents on the repos you want to summarize)
- A Slack workspace where you can install the agent's bot and invite it to one or more channels

<table>
  <tr>
    <td><strong>CHANNELS</strong></td>
    <td><code>slack</code> · <code>cron</code> — 5:30pm PT weekdays</td>
  </tr>
  <tr>
    <td><strong>CONNECTORS</strong></td>
    <td><code>github-mcp</code></td>
  </tr>
  <tr>
    <td colspan="2" align="center">
      <br />
      <a href="https://valet.dev/deploy?from=github.com/valet-agents/github-digests">
        <img src="https://raw.githubusercontent.com/valet-agents/github-digests/main/.github/deploy-button.svg" alt="Deploy Agent →" height="40" />
      </a>
      <br /><br />
    </td>
  </tr>
</table>
