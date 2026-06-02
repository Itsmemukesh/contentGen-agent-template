# contentGen Documentation Agent - Template

A customizable AI documentation agent for docs-as-code teams. Create Markdown documentation by combining context from GitHub, Figma, Jira, and Confluence—all driven by your repository conventions.

## What This Is

This is a **template** for building an AI agent that generates documentation automatically. Instead of writing from scratch, the agent:

1. **Researches** your GitHub issues, PRs, Figma designs, and specs
2. **Understands** your documentation folder structure and conventions
3. **Asks questions** to clarify scope, audience, tone
4. **Proposes structure** (outlines for approval before writing)
5. **Generates Markdown** validated against your style rules
6. **Produces ready-to-publish** documentation

## Quick Start (5 minutes)

```bash
# 1. Fork or copy this template to your repo
cp -r template_files/.github your-repo/.github
cp -r template_files/.vscode your-repo/.vscode

# 2. Configure MCPs (GitHub, Figma, etc.)
edit .vscode/mcp.json

# 3. Add authentication tokens
export GITHUB_TOKEN=your_token_here

# 4. Test it out
# Open .github/agents/contentGenTemplate.agent.md in VS Code
# Tell Copilot: "Create a guide for my new login feature from GitHub issue #456"
```

See [QUICKSTART.md](QUICKSTART.md) for detailed instructions.

## Features

### ✨ Smart Context Gathering
- **GitHub** - Issues, PRs, discussions, code context
- **Figma** - Design screenshots, UI labels, component names
- **Jira** - Ticket specs, acceptance criteria
- **Confluence** - Background documentation, business rules
- **Custom MCPs** - Extend with your own tools via SSE

### 🤔 Interactive Clarification
Agent asks structured questions to understand:
- Document type (new topic, revision, release notes, troubleshooting)
- Target audience (end users, developers, mixed)
- Tone and style preferences
- Special requirements (examples, diagrams, code snippets)

### 🔍 Content-Aware
- Searches existing docs to prevent duplication
- Suggests where new content should live
- Recommends consolidation of related docs
- Cross-references automatically

### ✅ Quality Validation
- Validates Markdown syntax
- Checks all links (internal and external)
- Enforces naming conventions
- Verifies required sections present
- Ensures consistency with existing style

### 📚 Multiple Doc Types
Templates for:
- **New topics** - Complete feature guides
- **Revisions** - Updates when UIs or workflows change
- **Release notes** - Feature announcements
- **Troubleshooting** - Problem-solution guides
- **Process docs** - Step-by-step procedures
- (Customize with your own types)

## How It Works

```
User request
    ↓
Agent asks clarifying questions (1-3 rounds)
    ↓
Agent gathers context (GitHub, Figma, Jira, etc.)
    ↓
Agent analyzes existing docs (content audit)
    ↓
Agent proposes outline/structure
    ↓
User approves (or requests changes)
    ↓
Agent writes Markdown
    ↓
Agent validates against style rules
    ↓
✅ Ready-to-publish documentation
```

## File Structure

```
template_files/
├── .github/
│   ├── agents/
│   │   └── contentGenTemplate.agent.md    # Agent definition
│   └── instructions/
│       └── (your custom rules here)
├── .vscode/
│   └── mcp.json                           # MCP configuration
├── skills/
│   ├── determine-content-scope/
│   ├── markdown-validation/
│   ├── github-aware-writing/
│   ├── interactive-question-ux/
│   └── content-audit/
├── examples/                              # Real workflow examples
│   ├── new-topic-example.md
│   ├── revision-example.md
│   ├── release-notes-example.md
│   └── (more examples)
├── templates/
│   └── CUSTOMIZE.md                       # Customization guide
└── README.md                              # This file
```

## Configuration

### 1. Copy Template

```bash
git clone your-repo
cp -r template_files/.github your-repo/.github
cp -r template_files/.vscode your-repo/.vscode
```

### 2. Edit `.vscode/mcp.json`

Configure which tools the agent uses:

```json
{
  "mcpServers": {
    "github": {
      "enabled": true,
      "config": {
        "owner": "your-org",
        "repo": "your-repo"
      }
    },
    "figma": {
      "enabled": true,  // Enable if design-driven
      "config": {
        "team_id": "your-figma-team-id"
      }
    }
  }
}
```

### 3. Set Environment Variables

```bash
export GITHUB_TOKEN=your_personal_access_token
export FIGMA_TOKEN=your_figma_token
# (etc. for other tools)
```

See [SETUP.md](SETUP.md) for detailed authentication setup.

### 4. Customize Skills

Edit skills in `.github/skills/` to match your:
- **Document types** - What doc types do you create?
- **Folder structure** - Where do docs live?
- **Naming conventions** - kebab-case, CamelCase?
- **Required sections** - What should every doc have?
- **Audience** - Who reads your docs?

See [CUSTOMIZATION.md](CUSTOMIZATION.md) for detailed customization guide.

## Usage Examples

### Example 1: Create New Guide from GitHub Issue

```
You: "Create a guide for GitHub issue #456 about two-factor authentication"

Agent:
  → Asks: Document type? Audience? Tone? Special requirements?
  → Fetches: GitHub issue details
  → Searches: Existing docs (no duplicate)
  → Proposes: Outline for approval
  → Writes: Complete Markdown guide
  → Validates: Syntax, links, style
  ✅ Result: /guides/security/two-factor-authentication.md
```

### Example 2: Update Docs After UI Change

```
You: "Update the login guide—UI changed in PR #789"

Agent:
  → Asks: What changed? Scope of changes?
  → Fetches: GitHub PR #789 details and new screenshots
  → Finds: Existing /guides/getting-started/login.md
  → Compares: Old vs. new UI/workflow
  → Proposes: What sections need updating
  → Updates: Relevant sections only
  ✅ Result: Updated login guide reflecting new UI
```

### Example 3: Write Release Notes

```
You: "Create release notes for v2.5.0"

Agent:
  → Asks: What's new? Breaking changes? Known issues?
  → Fetches: GitHub milestone v2.5.0 PRs
  → Fetches: Jira tickets for features
  → Organizes: Features, fixes, breaking changes
  → Writes: Professional release notes with links
  ✅ Result: /release-notes/v2.5.0.md ready to announce
```

## Benefits

| Benefit | How |
|---------|-----|
| **Faster** | No manual research, outline → publish in minutes |
| **Consistent** | Follows your repo's style rules automatically |
| **Accurate** | Sources context from actual code/design, not imagination |
| **Reusable** | Template works for any docs-as-code project |
| **Extensible** | Add custom MCPs, doc types, validation rules |
| **Maintainable** | Docs stay in sync with code via GitHub context |

## Comparison: Before & After

### Before (Manual)

```
1. Read GitHub issue carefully
2. Look at Figma design
3. Think about what to write
4. Write draft manually
5. Edit for style consistency
6. Check links work
7. Validate against conventions
8. Publish

⏱️ Time: 30-60 minutes for one guide
```

### After (With Agent)

```
1. Ask agent: "Create guide for GitHub issue #456"
2. Answer 3 clarifying questions
3. Approve proposed outline
4. Agent writes & validates automatically

⏱️ Time: 5-10 minutes for one guide
```

## Learn More

- [QUICKSTART.md](QUICKSTART.md) - Get started in 5 minutes
- [SETUP.md](SETUP.md) - Detailed authentication setup
- [CUSTOMIZATION.md](CUSTOMIZATION.md) - Extend for your needs
- [templates/CUSTOMIZE.md](templates/CUSTOMIZE.md) - Specific customization guide
- [examples/](examples/) - Real workflow walkthroughs

## Architecture

This template uses a 4-layer configuration-driven design:

```
Layer 1: .github/agents/
  → Agent behavior and workflow

Layer 2: .github/skills/
  → Reusable decision logic and patterns

Layer 3: .vscode/
  → External integrations (MCPs)

Layer 4: Your repo conventions
  → Folder structure, naming, templates define everything
```

**Key principle:** Repository conventions drive agent behavior, not hardcoded logic.

## Support

- **Questions?** See the [examples/](examples/) folder for real workflows
- **Stuck?** Check [CUSTOMIZATION.md](CUSTOMIZATION.md) for your specific scenario
- **Need help?** Create an issue in your fork with details

## Contributing

Have improvements? Consider:
- Adding new doc type templates
- Improving validation rules
- Adding examples for your use case
- Fixing edge cases

## License

This template is provided as-is for documentation teams. Customize freely.

## Key Files to Edit

When setting up:

1. **`.vscode/mcp.json`** - Configure your tools
2. **`.github/skills/determine-content-scope/SKILL.md`** - Define your doc types
3. **`.github/skills/markdown-validation/SKILL.md`** - Set style rules
4. **`.github/agents/contentGenTemplate.agent.md`** - Adjust workflow (usually not needed)

Everything else can stay as-is unless you need advanced customization.

---

**Ready?** Start with [QUICKSTART.md](QUICKSTART.md) for a 5-minute setup!
