# gh-agents

## Purpose
The `gh-agents` project is a framework for building OpenClaw skills that interact with GitHub using the GitHub App authentication flow. These skills can automate tasks like creating issues, updating pull requests, and more.

## Structure
- `agents/`: Contains pre-built agents configured to use your GitHub App.
- `skills/`: Where you'll add new skills (directories for each skill).
- `docs/`: Documentation for developers on how to create and contribute skills.

## Prerequisites
1. A GitHub organization with a registered GitHub App.
2. OpenClaw installed locally or in your environment.

## Quick Start: Adding a New Agent
1. Navigate into an `agents` directory that corresponds to your environment (e.g., `dev`, `prod`).
2. Create a new agent config YAML file (e.g., `myagent.yaml`) with the following structure:
   ```yaml
   name: "My GitHub App Agent"
   type: github_app_agent
   ```
3. Point OpenClaw to this agent using the appropriate environment variable or configuration.

## Example openclaw.json Snippet for GitHub App Auth
```json
{
  "auth": {
    "type": "github-app",
    "app_id": "<your-github-app-id>",
    "installation_id": "<installation-id>",
    "private_key": "-----BEGIN RSA PRIVATE KEY-----\n...\n-----END RSA PRIVATE KEY-----"
  }
}
```

## How to Contribute
1. Fork the repository.
2. Create a new skill in `skills/`.
3. Follow the tutorial in `docs/skill-tutorial.md` to structure your skill and write a script that interacts with GitHub using OpenClaw's GitHub App helper.
4. Package your skill as an archive (e.g., tar.gz) and submit a pull request.

## License
This project is open source under the MIT license.

---