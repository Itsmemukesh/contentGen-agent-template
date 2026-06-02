# SETUP: Detailed MCP Configuration

This guide walks through setting up each MCP (Model Context Protocol) integration for the contentGen agent.

## Overview

MCPs connect the agent to external tools:

| Tool | Purpose | Optional? | Setup Time |
|------|---------|-----------|-----------|
| **GitHub** | Fetch issues, PRs, discussions | ❌ Required | 5 min |
| **Figma** | Get design screenshots & UI labels | ✅ Optional | 5 min |
| **Jira** | Fetch tickets and specs | ✅ Optional | 5 min |
| **Confluence** | Read background docs | ✅ Optional | 5 min |
| **Draw.io** | Generate diagrams | ✅ Optional | 2 min |
| **Custom** | Your own tools via SSE | ✅ Optional | 10 min |

## GitHub (Required)

### Step 1: Create Personal Access Token

1. Go to https://github.com/settings/tokens
2. Click "Generate new token" → "Generate new token (classic)"
3. Fill in:
   - **Token name:** `contentgen-ai-agent`
   - **Expiration:** 90 days (or custom)
   - **Scopes:** Select exactly these:
     - ✅ `repo` - Full control of private repositories
     - ✅ `workflow` - Update GitHub Action workflows
     - ✅ `read:discussion` - Read access to discussions
4. Click "Generate token"
5. **Copy immediately** (you won't see it again!)

### Step 2: Store Token Securely

**Option A: Environment Variable (Recommended)**

Windows (Command Prompt):
```cmd
setx GITHUB_TOKEN your_token_here
```

Windows (PowerShell):
```powershell
[Environment]::SetEnvironmentVariable("GITHUB_TOKEN", "your_token_here", "User")
```

macOS/Linux:
```bash
export GITHUB_TOKEN=your_token_here
```

**Option B: .env File (For Development)**

Create `.env` in repo root:
```
GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Add to `.gitignore`:
```
.env
.env.local
*.env
```

### Step 3: Configure in .vscode/mcp.json

```json
{
  "mcpServers": {
    "github": {
      "enabled": true,
      "required": true,
      "config": {
        "owner": "your-org-or-username",
        "repo": "your-repo-name",
        "auth": "environment:GITHUB_TOKEN"
      },
      "environment": {
        "GITHUB_TOKEN": "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
      }
    }
  }
}
```

### Step 4: Test

In VS Code chat:
```
Test GitHub connection
```

Expected response:
```
✅ Connected to your-org/your-repo
Can fetch issues, PRs, and discussions
```

### Troubleshooting

**Error: 401 Unauthorized**
- Token expired or invalid
- Check scopes include `repo`, `workflow`, `read:discussion`
- Regenerate token

**Error: Repository not found**
- Wrong owner/repo name
- Check you have access to the repo
- Try with your username instead of org name

## Figma (Optional)

### When to Enable

Use Figma integration when:
- Your docs are UI-driven (guides, tutorials)
- Designers create mockups/flows you need to document
- You want screenshots automatically pulled from designs

### Step 1: Get Figma API Token

1. Go to https://www.figma.com/developers/api#access-tokens
2. Click "Create new personal access token"
3. Give it a name: `contentgen-agent`
4. Click "Generate token"
5. **Copy immediately**

### Step 2: Find Your Team ID

1. Go to Figma
2. Open a team project
3. Look at URL: `figma.com/team/YOUR_TEAM_ID/files`
4. Copy the team ID

### Step 3: Configure in .vscode/mcp.json

```json
{
  "mcpServers": {
    "figma": {
      "enabled": true,
      "config": {
        "team_id": "12345:abc...",
        "auth": "environment:FIGMA_TOKEN"
      },
      "environment": {
        "FIGMA_TOKEN": "figd_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
      }
    }
  }
}
```

### Step 4: Test

In VS Code chat:
```
Fetch the Figma design at figma.com/file/YOUR_FILE_ID
```

Expected: Agent returns design details (title, components, etc.)

### Usage

Reference Figma designs in requests:
```
Create a guide based on GitHub issue #456 and this Figma design:
https://www.figma.com/file/YOUR_FILE_ID
```

## Jira (Optional)

### When to Enable

Use Jira integration when:
- Issues are tracked in Jira
- Specs are stored as Jira tickets
- You want acceptance criteria in docs

### Step 1: Generate API Token

1. Go to https://id.atlassian.com/manage-profile/security/api-tokens
2. Click "Create API token"
3. Name: `contentgen-agent`
4. Click "Create"
5. **Copy token immediately**

### Step 2: Create Basic Auth String

You need: `Base64(username:api_token)`

**On Windows (PowerShell):**
```powershell
$credentials = "your-email@company.com:your_api_token"
$bytes = [System.Text.Encoding]::UTF8.GetBytes($credentials)
$encoded = [System.Convert]::ToBase64String($bytes)
Write-Host $encoded
```

**On macOS/Linux:**
```bash
echo -n "your-email@company.com:your_api_token" | base64
```

### Step 3: Configure in .vscode/mcp.json

```json
{
  "mcpServers": {
    "jira": {
      "enabled": true,
      "config": {
        "host": "https://your-domain.atlassian.net",
        "project_key": "DOCS",
        "auth": "environment:JIRA_AUTH"
      },
      "environment": {
        "JIRA_AUTH": "Basic base64_encoded_string_here"
      }
    }
  }
}
```

### Step 4: Test

In VS Code chat:
```
Find Jira issues with label 'documentation'
```

Expected: Lists relevant Jira tickets

### Usage

Reference Jira tickets in requests:
```
Create a guide from Jira ticket DOCS-456 and GitHub issue #123
```

## Confluence (Optional)

### When to Enable

Use Confluence when:
- Business rules are documented on Confluence
- Feature specs are on Confluence pages
- Background context lives on Confluence

### Step 1: Generate API Token

(Same as Jira - they use the same auth)

Go to https://id.atlassian.com/manage-profile/security/api-tokens

### Step 2: Create Basic Auth String

(Same as Jira)

```
Base64(email:api_token)
```

### Step 3: Configure in .vscode/mcp.json

```json
{
  "mcpServers": {
    "confluence": {
      "enabled": true,
      "config": {
        "host": "https://your-domain.atlassian.net/wiki",
        "auth": "environment:CONFLUENCE_AUTH"
      },
      "environment": {
        "CONFLUENCE_AUTH": "Basic base64_encoded_string_here"
      }
    }
  }
}
```

### Step 4: Test

In VS Code chat:
```
Find Confluence pages about authentication
```

### Usage

```
Create a guide using:
- GitHub issue #456
- Confluence page "Authentication Spec"
```

## Draw.io (Optional)

### When to Enable

Use Draw.io when:
- You need to generate diagrams in documentation
- Flowcharts, architecture diagrams, etc.
- Part of documentation generation workflow

### Configuration

Draw.io is a built-in MCP - minimal setup needed:

```json
{
  "mcpServers": {
    "drawio": {
      "enabled": true,
      "config": {
        "api_endpoint": "https://drawio-mcp.example.com"
      }
    }
  }
}
```

### Usage

In your documentation requests:
```
Create a guide with a flowchart showing the authentication process
```

Agent will generate diagram code and embed in documentation.

## Custom MCP (Advanced)

### When to Use

Extend with your own tools:
- Internal wikis or APIs
- Custom databases
- Legacy documentation systems
- Proprietary tools

### Pattern: Server-Sent Events (SSE)

Custom MCPs use SSE (Server-Sent Events) pattern.

**Your server needs:**
1. HTTP endpoint
2. Support for SSE streaming
3. Request/response format matching MCP spec

### Configuration

```json
{
  "mcpServers": {
    "my-custom-tool": {
      "type": "sse",
      "enabled": true,
      "config": {
        "endpoint": "http://localhost:3000/mcp",
        "auth": "environment:CUSTOM_MCP_AUTH"
      },
      "environment": {
        "CUSTOM_MCP_AUTH": "Bearer your_token_here"
      }
    }
  }
}
```

### Example: Simple Node.js MCP Server

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  if (req.url === '/mcp' && req.method === 'POST') {
    res.writeHead(200, { 'Content-Type': 'text/event-stream' });
    
    let data = '';
    req.on('data', chunk => data += chunk);
    req.on('end', () => {
      // Parse request, process, send response
      res.write('data: {"status": "success", "data": {...}}\n\n');
      res.end();
    });
  }
});

server.listen(3000);
```

See [Model Context Protocol spec](https://spec.modelcontextprotocol.io) for full details.

## Environment Variable Priority

Agent loads environment variables in this order:

1. **System environment variables** (highest priority)
2. **.env file** in repo root
3. **.vscode/mcp.json** (inline, lowest priority)

**Best practice:** Use system environment variables for production, .env for development.

## Testing All MCPs

Create a test in VS Code chat:

```
Run diagnostics
Show which MCPs are connected
```

Expected output:
```
✅ GitHub - Connected to your-org/your-repo
✅ Figma - Connected to team "Design"
⏳ Jira - Not configured (optional)
⏳ Confluence - Not configured (optional)
✅ Draw.io - Available
```

## Troubleshooting

### MCP Connection Refused

```
Error: Cannot connect to MCP endpoint
```

**Solutions:**
1. Check server is running (if custom MCP)
2. Verify endpoint URL in config
3. Check firewall/network access
4. Ensure auth token is valid

### Token Expired

```
Error: 401 Unauthorized
```

**Solutions:**
1. Regenerate token (GitHub, Figma, Jira, Confluence)
2. Update environment variable
3. Test connection

### Agent Not Using MCP

```
Agent ignores your GitHub issue request
```

**Solutions:**
1. Check MCP is enabled in .vscode/mcp.json
2. Verify token is set (echo $GITHUB_TOKEN)
3. Check MCP is marked as "enabled": true
4. Restart VS Code

## Security Best Practices

1. **Never commit tokens to git**
   - Add `.env` to `.gitignore`
   - Use environment variables

2. **Use fine-grained tokens when possible**
   - GitHub: Fine-grained personal access tokens
   - Jira: API tokens instead of passwords

3. **Rotate tokens regularly**
   - GitHub: Every 90 days recommended
   - Figma: Every 6 months
   - Jira: Every 90 days

4. **Scope permissions minimally**
   - GitHub: Only `repo`, `workflow`, `read:discussion`
   - Don't grant `admin:*` scopes

5. **Monitor token usage**
   - GitHub: Check "Personal access tokens" page
   - Jira: Check API token activity

## Next Steps

- ✅ Set up GitHub (required)
- 📝 Add Figma (if design-driven)
- 🎫 Add Jira (if ticket-based)
- 📚 Add Confluence (if spec-based)
- 🔌 Add custom MCP (if needed)

See [QUICKSTART.md](QUICKSTART.md) for a quick setup, or [README.md](README.md) for overview.
