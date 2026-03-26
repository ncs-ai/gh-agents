# Creating a New OpenClaw Skill for GitHub Actions

## Overview
This tutorial will guide you through creating a new skill that uses your GitHub App to perform actions like creating issues in repositories.

## Prerequisites
- A GitHub organization with a registered GitHub App.
- Basic familiarity with shell scripting and YAML configuration.

## Step 1: Directory Layout
Create the following directory structure:
```
skills/
└── my-github-skill/
    ├── skill.yaml
    └── create_issue.sh
```

## Step 2: skill.yaml Essentials
Define your skill's metadata in `skill.yaml`:
```yaml
name: "My GitHub Skill"
description: "Creates issues using the GitHub App JWT helper."
author: "Your Name <your@email.com>"
version: "0.1.0"
entrypoint: create_issue.sh
dependencies:
  - github-app-jwt-helper
```

## Step 3: Script Example (create_issue.sh)
```bash
#!/bin/bash

# Load OpenClaw environment variables
source /path/to/openclaw/env

# Ensure we're using GitHub App auth
AUTH_TYPE="github-app"
APP_ID="<your-github-app-id>"
INSTALLATION_ID="<installation-id>"

# Function to create an issue via the GitHub API
create_issue() {
  local repo=$1
  local title=$2
  local body=$3

  curl -X POST "https://api.github.com/repos/$repo/issues" \
    -H "Authorization: Bearer $(jwt_helper get_token)" \
    -d "{\"title\":\"$title\",\"body\":\"$body\"}"
}

# Example usage
create_issue "org/repo" "Test Issue" "This is an automated issue."
```

## Step 4: Test Locally
1. Make sure the `github-app-jwt-helper` dependency is installed in your OpenClaw environment.
2. Run:
   ```bash
   openclaw run skill.yaml create_issue.sh org/repo "Test Issue" "Body of the issue."
   ```

## Step 5: Package and Use
1. Archive your skill directory as a tarball.
2. Deploy to your environment or organization's OpenClaw instance.

## Next Steps
Explore additional GitHub App capabilities in the OpenClaw documentation for more advanced automation.