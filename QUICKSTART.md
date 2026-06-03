# QUICKSTART: Get Started in 5 Minutes

Get the contentGen agent running in your documentation repository in under 5 minutes.

## Prerequisites

- VS Code with GitHub Copilot extension
- A GitHub repository (public or private)
- Personal GitHub token (classic or fine-grained)

## Step 1: Copy Template (1 min)

Copy the template files to your repo:

```bash
# Clone or download contentGen-agent-template/
cd your-repo

# Copy template structure
cp -r contentGen-agent-template/.github ./.github
cp -r contentGen-agent-template/.vscode ./.vscode
cp contentGen-agent-template/README.md ./AI_DOCS_README.md  # Optional: reference copy
```

Your repo now has:
- `.github/agents/contentGenTemplate.agent.md` - Agent definition
- `.github/skills/` - Reusable decision logic
- `.vscode/mcp.json` - MCP configuration

## Step 2: Create GitHub Token (1 min)

Generate a personal access token:

1. Go to: https://github.com/settings/tokens
2. Click "Generate new token" → "Generate new token (classic)"
3. Set:
   - **Name:** `contentgen-ai-agent`
   - **Scopes:** Select:
     - ✅ `repo` (full control of private repos)
     - ✅ `workflow` (read/write GitHub Actions)
     - ✅ `read:discussion` (read discussions)
4. Click "Generate token"
5. **Copy token immediately** (you won't see it again)

## Step 3: Set Environment Variable (1 min)

Add token to your system environment:

### Windows (Command Prompt)
```cmd
setx GITHUB_TOKEN your_token_here_paste_copied_token
```

### Windows (PowerShell)
```powershell
[Environment]::SetEnvironmentVariable("GITHUB_TOKEN", "your_token_here_paste_copied_token", "User")
```

### macOS / Linux
```bash
export GITHUB_TOKEN=your_token_here_paste_copied_token
```

**Or use `.env` file (safer for secrets):**

Create `.env` in your repo root:
```
GITHUB_TOKEN=your_token_here
```

Add to `.gitignore`:
```
.env
.env.local
```

## Step 4: Configure MCP (1 min)

Edit `.vscode/mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "enabled": true,
      "config": {
        "owner": "YOUR_ORG_OR_USERNAME",    // ← Change this
        "repo": "YOUR_REPO_NAME"             // ← Change this
      }
    },
    "figma": {
      "enabled": false,  // Enable later if needed
      ...
    }
  }
}
```

Replace with your actual organization and repo name.

## Step 5: Test It Out (1 min)

Open VS Code and test the agent:

1. Open `.github/agents/contentGenTemplate.agent.md`
2. In the GitHub Copilot chat:
   ```
   Create a guide for a new login feature based on GitHub issue #1
   ```
3. Agent will:
   - Ask clarifying questions
   - Fetch GitHub issue #1
   - Generate documentation outline
   - Create Markdown file

✅ **If successful, you're ready to go!**

## Next Steps

### Customize for Your Project

Edit these files to match your documentation style:

1. **Document types:** `.github/skills/determine-content-scope/SKILL.md`
   - Add/remove: new-topic, revision, release-notes, faq, api-docs, etc.

2. **Folder structure:** `.github/skills/content-audit/SKILL.md`
   - Update: Where your docs live (/guides/, /docs/, /reference/)
   - Update: Naming conventions (kebab-case, CamelCase, etc.)

3. **Validation rules:** `.github/skills/markdown-validation/SKILL.md`
   - Update: Required sections for each doc type
   - Update: Line length, heading structure, etc.

4. **Audience & tone:** `.github/agents/contentGenTemplate.agent.md`
   - Update: Supported doc types
   - Update: Default audience/tone

See [CUSTOMIZATION.md](CUSTOMIZATION.md) for detailed customization.

### Add More Tools (Optional)

Enable additional context sources in `.vscode/mcp.json`:

**Figma (for design-driven docs):**
```json
{
  "figma": {
    "enabled": true,
    "config": {
      "team_id": "YOUR_FIGMA_TEAM_ID"
    },
    "environment": {
      "FIGMA_TOKEN": "your_figma_token_here"
    }
  }
}
```

**Jira (for ticket-based docs):**
```json
{
  "jira": {
    "enabled": true,
    "config": {
      "host": "https://your-domain.atlassian.net",
      "project_key": "DOCS"
    },
    "environment": {
      "JIRA_AUTH": "Basic base64_encoded_creds"
    }
  }
}
```

See [SETUP.md](SETUP.md) for detailed MCP setup.

### Learn by Example

Check the [examples/](examples/) folder:

- `new-topic-example.md` - Create a feature guide
- `revision-example.md` - Update docs after UI change
- `release-notes-example.md` - Write release announcements
- (More examples coming)

Each example shows the full agent workflow with real requests and outputs.

## Troubleshooting

### "Token authentication failed"

```
Error: GitHub API returned 401 Unauthorized
```

**Fix:**
1. Verify token is valid: `curl -H "Authorization: token YOUR_TOKEN" https://api.github.com/user`
2. Check token has right scopes (needs `repo`, `workflow`, `read:discussion`)
3. Regenerate token if expired

### "Cannot find repository"

```
Error: Repository not found
```

**Fix:**
1. Double-check owner/repo in `.vscode/mcp.json`
2. Verify you have access to the repo
3. Try with `owner: your-username` first (not organization)

### "Agent asks generic questions instead of GitHub context"

```
Agent: "What do you need documented?" (not fetching GitHub issue)
```

**Fix:**
1. Check GITHUB_TOKEN is set: `echo $GITHUB_TOKEN` (or `$env:GITHUB_TOKEN` in PowerShell)
2. Verify MCP config has GitHub enabled and configured
3. Try mentioning the issue directly: "Document GitHub issue #123"

### "Copilot chat not showing agent options"

**Fix:**
1. Reload VS Code (Ctrl+Shift+P → "Developer: Reload Window")
2. Check GitHub Copilot extension is updated
3. Ensure `.github/agents/contentGenTemplate.agent.md` exists
4. Restart VS Code

## Common Commands

### Run the Agent

```
Chat: "Create a guide for GitHub issue #456"
Chat: "Update the login documentation from PR #789"
Chat: "Write release notes for v2.5.0"
Chat: "Document the new API authentication endpoint"
```

### Get Help Within Chat

```
Chat: "What document types can you create?"
Chat: "Show me an example of a new topic"
Chat: "How do I customize the agent?"
```

### Check Configuration

```
Chat: "Can you access GitHub issues?"
Chat: "What MCP tools are available?"
Chat: "Show me the folder structure"
```

## What's Next?

1. ✅ **Generated your first guide?** Great!
2. 📝 **Customize** the agent for your docs structure
3. 🔌 **Add more tools** (Figma, Jira, Confluence)
4. 📚 **Build library** of examples for your team
5. 🚀 **Scale up** - Use for all documentation

## Support

- **How do I customize?** → [CUSTOMIZATION.md](CUSTOMIZATION.md)
- **How do I add Figma/Jira?** → [SETUP.md](SETUP.md)
- **What can the agent do?** → [README.md](README.md)
- **Show me examples** → [examples/](examples/)

---

**Done!** Your contentGen agent is ready. 🎉

Start with: "Create a guide for GitHub issue #1"
