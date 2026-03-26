# Creating a New OpenClaw Skill for GitHub App

This tutorial walks through creating a custom skill that uses the GitHub App to perform actions (e.g., create an issue).

## Skill Layout

```
my-skill/
├── skill.yaml
├── scripts/
│   └── create_issue.sh
└── README.md
```

## skill.yaml Essentials

```yaml
name: github-create-issue
description: Create a GitHub issue using the installed GitHub App
version: 0.1.0
agent: openclaw
entrypoint: scripts/create_issue.sh
parameters:
  - name: repo
    type: string
    description: Repository name (e.g., ncs-ai/my-repo)
  - name: title
    type: string
    description: Issue title
  - name: body
    type: string
    description: Issue body
```

## Example Script (bash)

```bash
#!/usr/bin/env bash
set -euo pipefail

# Load helper to get installation token
TOKEN=$(./github_jwt.sh "$CLIENT_ID" "$PEM" | {
  JWT=$(cat)
  INSTALL_ID=$(curl -s -H "Authorization: Bearer $JWT" https://api.github.com/app/installations | python3 -c "import json,sys; print([i['id'] for i in json.load(sys.stdin) if i.get('account',{}).get('login')=='$ORG'][0])")
  curl -s -X POST -H "Authorization: Bearer $JWT" https://api.github.com/app/installations/$INSTALL_ID/access_tokens | python3 -c "import json,sys; print(json.load(sys.stdin)['token'])"
})

REPO="{{repo}}"
TITLE="{{title}}"
BODY="{{body}}"

curl -X POST "https://api.github.com/repos/$REPO/issues" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Accept: application/vnd.github+json" \
  -d "$(jq -n --arg t "$TITLE" --arg b "$BODY" '{title:$t, body:$b}')"
```

## Testing Locally

1. Ensure `github_jwt.sh` and the app PEM are available in the agent workspace.
2. Export `CLIENT_ID`, `PEM`, `ORG` environment variables.
3. Run: `./scripts/create_issue.sh --repo ncs-ai/gh-agents --title "Test" --body "Hello"`

## Packaging & Use

- Commit the skill folder under `skills/` in your agent repo.
- In your agent’s `openclaw.json`, add the skill to the agent’s allowed skills or global skills.
- The agent can now invoke the skill by name.

---
_Last updated: 2026-03-26 by Spark_
