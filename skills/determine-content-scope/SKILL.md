# Skill: Determine Content Scope

## Purpose

Clarify what type of documentation needs to be created or revised. This skill disambiguates user intent and helps the agent choose the right template and workflow.

## What This Skill Does

Asks structured questions to determine:
- **Document type** - new topic, revision, release notes, troubleshooting, process doc, etc.
- **Scope** - single feature, multi-feature, entire workflow
- **Context source** - GitHub issue, Figma design, Jira ticket, discussion, specification
- **Update impact** - new doc, update existing, replace, consolidate with another

## How to Use

When a user says: *"I need to document feature X"*

Agent asks:
1. Is this a **new topic** or revising an **existing doc**?
2. What's the primary context? (GitHub issue / Figma design / Jira ticket / Email / Other)
3. Is this a **single feature** or **multiple features**?
4. Should this be a **standalone doc** or **part of a larger guide**?

Based on answers, agent determines workflow.

## Document Types (Customize This)

| Type | When to Use | Template | Audience |
|------|-----------|----------|----------|
| `new-topic` | Documenting a new feature | Full guide with examples | Users/developers |
| `revision` | Updating existing docs (UI changed) | Update sections only | Users/developers |
| `release-notes` | Announcing new features/fixes | Bullet lists, links | All stakeholders |
| `troubleshooting` | How to solve common issues | Problem → Solution | Users |
| `process-doc` | Workflow or procedure | Step-by-step with screenshots | Team/users |
| `faq` | Frequently asked questions | Q&A format | Users |
| (Add your own) | | | |

## Decision Tree

```
START: "What do you need documented?"
│
├─ "New feature" → new-topic
│   ├─ "Single feature" → create one file
│   └─ "Multiple features" → create index + individual files
│
├─ "Docs are outdated" → revision
│   ├─ "Small update" → edit sections
│   └─ "Major rewrite" → full revision
│
├─ "Release announcements" → release-notes
│   ├─ "This sprint" → add to existing
│   └─ "New release" → create new file
│
├─ "Users are confused" → troubleshooting
│   ├─ "New issue" → add new section
│   └─ "Common pattern" → create dedicated guide
│
└─ "Document a workflow" → process-doc
```

## Questions to Ask (Use `vscode_askQuestions`)

```yaml
questions:
  - header: doc_type
    question: What type of document are you creating?
    options:
      - label: New topic
        description: Documenting a new feature/capability
      - label: Revision
        description: Updating existing documentation
      - label: Release notes
        description: Announcing changes for a release
      - label: Troubleshooting guide
        description: How to solve a problem
      - label: Process/workflow documentation
        description: Step-by-step procedure

  - header: context_source
    question: What's your primary context source?
    options:
      - label: GitHub issue
      - label: Figma design
      - label: Jira ticket
      - label: Confluence/Spec document
      - label: Customer feedback
      - label: Email/Slack thread

  - header: scope
    question: How many features/topics?
    options:
      - label: Single feature
      - label: Multiple related features
      - label: Complete workflow

  - header: update_type
    question: Is this new content or updating existing?
    options:
      - label: Completely new
      - label: Update specific sections
      - label: Full rewrite
      - label: Consolidate multiple docs
```

## Agent Logic

After determining scope:

```
IF doc_type == "new-topic":
  USE Template: new-topic-template.md
  GATHER: Figma designs + GitHub issues
  PROPOSE: Full outline with sections
  OUTPUT: Single .md file in /guides/ folder

IF doc_type == "revision":
  USE Template: revision-guide-template.md
  FIND: Existing doc path
  GATHER: What changed (GitHub PR / Figma update)
  PROPOSE: Which sections to update
  OUTPUT: Updated .md file in same location

IF doc_type == "release-notes":
  USE Template: release-notes-template.md
  GATHER: PRs merged, bugs fixed, features shipped
  PROPOSE: Sections: New Features / Fixes / Breaking Changes
  OUTPUT: New release-notes entry

IF doc_type == "troubleshooting":
  USE Template: troubleshooting-template.md
  GATHER: User issues, error messages, support tickets
  PROPOSE: Problem → Cause → Solution format
  OUTPUT: New troubleshooting guide or section
```

## Output Format

Agent returns to user:

```
✅ SCOPE DETERMINED:
  📝 Type: new-topic
  🎯 Context: GitHub issue #456, Figma design
  📊 Scope: Single feature
  📂 Location: /guides/feature-name.md

Ready to proceed with research & outline?
```

## Customization Notes

1. **Add your doc types** - Modify the table above to match your needs
2. **Adjust questions** - Add/remove options based on your workflows
3. **Customize templates** - Each doc type should have its own template
4. **Define audience** - Add audience info to guide tone & complexity

## Examples

### Example 1: New Feature Documentation
```
User: "I need to document our new auth system"
Agent asks:
  - Type? → "New topic"
  - Context? → "GitHub issue + Figma design"
  - Scope? → "Single feature"
Result:
  ✅ Determined: new-topic + guides/authentication.md
  → Gathers GitHub issue details + Figma screenshots
  → Creates comprehensive authentication guide
```

### Example 2: Updating Docs After UI Change
```
User: "The login UI changed, docs are outdated"
Agent asks:
  - Type? → "Revision"
  - What changed? → "GitHub PR #789"
  - Update sections? → "Screenshots + steps"
Result:
  ✅ Determined: revision + guides/getting-started.md
  → Compares old vs. new from GitHub PR
  → Updates only affected sections
  → Keeps rest of doc intact
```

### Example 3: Release Notes
```
User: "Can you write release notes for v2.5?"
Agent asks:
  - Type? → "Release notes"
  - What's new? → "2 features, 5 fixes"
  - Any breaking changes? → "Yes, one"
Result:
  ✅ Determined: release-notes + changelog.md
  → Gathers merged PRs + commits
  → Structures as: Features / Fixes / Breaking Changes
  → Adds links to relevant docs
```

## Related Skills

- **content-audit** - Check if this doc already exists or should be merged
- **github-aware-writing** - Format content based on GitHub context
- **interactive-question-ux** - Ask follow-up questions if needed
