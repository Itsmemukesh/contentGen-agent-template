# CUSTOMIZATION: Adapt for Your Team

This guide walks through customizing the contentGen agent for your specific documentation system, team, and requirements.

## What to Customize

Four key areas drive agent behavior:

| Area | File | Examples |
|------|------|----------|
| **Document Types** | `skills/determine-content-scope/SKILL.md` | Add: faq, tutorial, troubleshooting |
| **Folder Structure** | `skills/content-audit/SKILL.md` | Change: /docs/ to /reference/, etc. |
| **Validation Rules** | `skills/markdown-validation/SKILL.md` | Update: line length, required sections |
| **Workflow** | `agents/contentGenTemplate.agent.md` | Adjust: question flow, tone |

## 1. Add Custom Document Types

### Edit: `.github/skills/determine-content-scope/SKILL.md`

**Find this section:**
```markdown
## Document Types (Customize This)

| Type | When to Use | Template | Audience |
|------|-----------|----------|----------|
| `new-topic` | ... | ... | ... |
| `revision` | ... | ... | ... |
| `release-notes` | ... | ... | ... |
| `troubleshooting` | ... | ... | ... |
| `process-doc` | ... | ... | ... |
| (Add your own) | | | |
```

**Add your document types:**

```markdown
| `faq` | Frequently asked questions | Q&A format | Users |
| `api-docs` | API endpoint documentation | Reference | Developers |
| `tutorial` | Learning path / guided tour | Tutorial | New users |
| `glossary` | Term definitions | Definition list | Everyone |
| `decision-log` | Engineering decisions | Problem → Decision | Team |
```

### Update Decision Tree

Add to the decision tree:

```markdown
├─ "How-to FAQ" → faq
│   ├─ "Single question" → add to existing FAQ
│   └─ "New question set" → create new FAQ
│
├─ "API Documentation" → api-docs
│   ├─ "Single endpoint" → add to reference
│   └─ "New API version" → create new docs
```

### Update Questions

Add question to ask users about doc type:

```yaml
options:
  - label: New topic
  - label: Revision
  - label: FAQ
  - label: API documentation    # ← Add this
  - label: Tutorial
```

## 2. Define Folder Structure

### Edit: `.github/skills/content-audit/SKILL.md`

**Update the folder structure template:**

```markdown
## Folder Structure Template

Knowing your repo structure helps with audit:

\`\`\`
/docs/                        # ← Your docs root
  /guides/                    # ← User how-to guides
  /api/                       # ← API reference
  /tutorials/                 # ← Learning paths
  /faq/                       # ← Questions & answers
  /reference/                 # ← Technical specs
  /architecture/              # ← System design docs
\`\`\`
```

**If your structure is different:**

```markdown
\`\`\`
/help/                        # Your root
  /user-guides/               # How-tos for users
  /developer-docs/            # Developer reference
  /getting-started/           # Onboarding
  /troubleshooting/           # Problem solving
  /faq/                        # Q&A
\`\`\`
```

### Add Folder Mapping

```markdown
## Folder Mapping for Audit

When auditing, search in relevant folders first:

- **New feature?** → Search \`/user-guides/\` first
- **API change?** → Search \`/developer-docs/api/\`
- **User problem?** → Search \`/troubleshooting/\`
- **Release?** → Search \`/releases/\`
- **Training?** → Search \`/getting-started/\`
```

### Update Content-Aware Suggestions

```markdown
IF doc_type == "new-topic":
  LOCATION: /docs/guides/feature-name.md        # ← Update path
  USE Template: feature-guide-template.md

IF doc_type == "revision":
  LOCATION: Update existing file in same folder
  USE Template: revision-update-template.md

IF doc_type == "api-docs":
  LOCATION: /docs/api/endpoint-name.md          # ← Add new type
  USE Template: api-reference-template.md
```

## 3. Set Validation Rules

### Edit: `.github/skills/markdown-validation/SKILL.md`

**Update file naming conventions:**

```markdown
### 4. File Naming Conventions

Standard pattern (customize for your repo):
- Use PascalCase: `MyFeature.md` (matches your team style)
- Lowercase first letter: `myFeature.md` (camelCase variant)
- Underscores allowed: `my_feature.md`
- Max 50 characters
```

**Update required sections by doc type:**

```markdown
### 5. Required Sections (by doc type)

**New Topic:**
- [ ] Title (h1)
- [ ] Overview/introduction
- [ ] Prerequisites (if applicable)
- [ ] Steps or sections
- [ ] Examples
- [ ] Related reading        # ← Add custom requirement

**FAQ:**
- [ ] Category/topic heading
- [ ] Question in h3
- [ ] Answer paragraph
- [ ] Related questions list # ← Custom requirement

**API Docs:**
- [ ] Endpoint path (GET/POST/etc.)
- [ ] Parameters table
- [ ] Request example
- [ ] Response example
- [ ] Error codes
- [ ] Rate limits              # ← Add if needed
```

**Update line length:**

```markdown
### 6. Line Length & Formatting

Standards:
- Max 120 characters per line (if your team prefers wider)
- One blank line between sections
- Two blank lines before new h2 section
```

**Update heading structure:**

```markdown
### 2. Heading Structure

Rules:
- Start with `# Title` (only one h1 per file)
- Use `##` for main sections
- Use `###` for subsections
- Skip `####` for your docs (only 3 levels max)
```

## 4. Customize Agent Workflow

### Edit: `.github/agents/contentGenTemplate.agent.md`

**Update supported document types:**

```markdown
## Standard Workflow

### Phase 1: Clarification
Agent asks user via `vscode_askQuestions`:
\`\`\`
1. What type of document? (new-topic/revision/release-notes/faq/api-docs/tutorial)
2. What's the primary context? (GitHub issue/Figma design/Jira ticket/etc.)
3. Who is the audience? (end users/developers/product team/mixed)
\`\`\`
```

**Add your workflow phases if needed:**

```markdown
### Phase X: Content Modeling
Agent extracts:
- Key concepts
- User personas affected
- Success metrics
```

**Update audience options:**

```markdown
The audience typically includes:
- **End users** - Product users performing tasks
- **Developers** - Building integrations or extensions
- **Product team** - Stakeholders making decisions
- **Support team** - Helping customers
- **Mixed audience** - Everyone above
- (Add your personas) - Your organization-specific roles
```

## 5. Create Custom Templates

### Add to `.github/skills/determine-content-scope/SKILL.md`

Create template examples for each doc type:

```markdown
## Agent Logic

...existing logic...

IF doc_type == "faq":
  USE Template: faq-template.md
  GATHER: Customer support questions
  PROPOSE: Q&A format with tags
  OUTPUT: FAQ section or standalone file

IF doc_type == "tutorial":
  USE Template: tutorial-template.md
  GATHER: Step-by-step process
  PROPOSE: Learning path with sections
  OUTPUT: Multi-part tutorial guide
```

### Create Template Files

Save templates in `.github/templates/`:

**faq-template.md:**
```markdown
# FAQ: Topic Name

## Category Name

### Q: User question here?
A: Answer the question clearly.

### Q: Another common question?
A: Provide helpful answer with examples.
```

**tutorial-template.md:**
```markdown
# Tutorial: Learning Path Name

## Part 1: Introduction
[Prerequisites and overview]

## Part 2: First Steps
[Basic concepts]

## Part 3: Intermediate
[Building on basics]

## Part 4: Advanced
[Complex scenarios]
```

## 6. Configure Team Preferences

### Edit: `.github/agents/contentGenTemplate.agent.md`

**Set default tone:**

```markdown
## Customization for Your Repo

When you fork this template, customize:

### 5. Audience & Tone
Define your documentation style:
- Audience: Product users, developers, support team, mixed
- Tone: Conversational and friendly, professional, technical and precise
- Examples vs. text balance: 60% examples, 40% text
- Code snippet style: Always provide language-specific examples
```

**Set default audience:**

```markdown
### Examples vs. Text
Most teams prefer:
- **User guides:** 40% text, 60% examples/screenshots
- **API docs:** 30% text, 70% examples/code
- **Tutorials:** 50% text, 50% hands-on steps
- (Customize for your team)
```

## 7. Add Team-Specific Rules

### Create `.github/instructions/doc-standards.instructions.md`

Add custom validation rules:

```markdown
# Custom Documentation Standards

## Terminology

Use consistent terms across all docs:
- "Sign in" not "log in"
- "Dashboard" not "console"
- "Permissions" not "roles" (unless RBAC context)

## Examples Required

Every how-to guide must include:
1. Real-world use case
2. Code example or screenshot
3. Expected result

## Links

Every reference link must:
- Use descriptive text (not "here" or "click here")
- Link to appropriate doc level
- Include link context in parentheses

## Code Style

- All code examples must be tested
- Must specify language in code block
- Include comments for non-obvious sections
```

### Add Grammar & Style Rules

```markdown
## Writing Standards

### Voice
- Use active voice: "You can enable 2FA" not "2FA can be enabled"
- Address reader directly: "You'll see" not "Users will see"
- Conversational but professional

### Capitalization
- Sentence case for headings (not Title Case)
- Product names: Always capitalize (GitHub, Figma, etc.)

### Numbers
- Zero through ten: spell out
- 11+: use numerals

### Lists
- Parallel structure in all list items
- If one item ends with period, all do
- Use hyphens (-) not asterisks or bullets
```

## 8. Test Your Customizations

After customizing, test with real requests:

### Test 1: New Document Type

```
Chat: "Create a FAQ about authentication"
```

Expected: Agent recognizes new document type, asks appropriate questions

### Test 2: Folder Audit

```
Chat: "Create a new developer guide"
```

Expected: Agent suggests correct folder location based on your structure

### Test 3: Validation

```
Chat: "Check if my documentation follows standards"
```

Expected: Agent validates against custom rules

## Common Customization Examples

### Example 1: SaaS Product Team

**Customize:**
```yaml
Doc Types: [new-feature, revision, release-notes, api-docs, integration-guide, troubleshooting]
Folders: [/docs/features/, /docs/api/, /docs/guides/, /docs/troubleshooting/]
Audience: [End users, Developers, API consumers]
Tone: Conversational, examples-heavy, assume technical knowledge
Sections: Overview, Benefits, How to use, Examples, API reference
```

### Example 2: Open Source Project

**Customize:**
```yaml
Doc Types: [new-feature, revision, release-notes, contributing, faq, roadmap]
Folders: [/docs/, /docs/guides/, /docs/api/, /docs/contributing/]
Audience: [End users, Contributors, Maintainers]
Tone: Technical, precise, community-focused
Sections: Description, Requirements, Installation, Usage, Contributing
```

### Example 3: Enterprise Software

**Customize:**
```yaml
Doc Types: [administrator-guide, user-guide, release-notes, integration, compliance]
Folders: [/docs/admin/, /docs/users/, /docs/integrations/, /docs/compliance/]
Audience: [Administrators, End users, Integrators, Compliance teams]
Tone: Professional, formal, comprehensive
Sections: Overview, Prerequisites, Step-by-step, Screenshots, Troubleshooting, FAQ
```

## Checklist: Customization Complete ✅

- [ ] Updated document types in `determine-content-scope/SKILL.md`
- [ ] Updated folder structure in `content-audit/SKILL.md`
- [ ] Updated validation rules in `markdown-validation/SKILL.md`
- [ ] Updated workflow in `agents/contentGenTemplate.agent.md`
- [ ] Created custom templates for your doc types
- [ ] Set team preferences (tone, audience, examples)
- [ ] Added team-specific rules (terminology, standards)
- [ ] Tested with real requests
- [ ] Got team feedback and refined

## Next Steps

1. **Deploy to your team** - Share customized template
2. **Get feedback** - Run through real documentation tasks
3. **Iterate** - Refine rules based on output quality
4. **Scale** - Use across all documentation projects

## Advanced Customization

### Custom MCPs

If you want agent to integrate with internal tools:
- See [SETUP.md](SETUP.md) for custom MCP guide
- Requires HTTP server with SSE support
- Examples: internal wikis, custom databases, legacy systems

### Custom Skills

Create new skills for specialized needs:
- Add file: `.github/skills/my-custom-skill/SKILL.md`
- Document decision logic and examples
- Agent loads automatically

### Multi-Language Support

Customize for multiple languages:
- Create separate questions for each language
- Customize validation rules per locale
- Add language detection in agent workflow

See agent SKILL files for advanced patterns.

---

**Need help?** See [README.md](README.md) for overview or [QUICKSTART.md](QUICKSTART.md) for quick setup.
