# Skill: Interactive Question UX

## Purpose

Guide the agent to ask users clarifying questions through structured, user-friendly questions using the `vscode_askQuestions` tool. This ensures the agent gathers necessary context before proceeding with documentation work.

## When to Use This Skill

Use structured questions when:
- ❓ The user's intent is ambiguous
- ❓ Required information is missing
- ❓ Multiple valid paths exist
- ❓ Critical decisions need user input
- ❓ Assumptions would lead to wrong output

**Don't ask if:**
- ✅ Context is clear from issue/design
- ✅ Defaults would work fine
- ✅ User already provided the info

## Question Types

### Type 1: Single Select (Pick One)

Use when: User must choose from predefined options.

```yaml
header: doc_type
question: What type of document are you creating?
options:
  - label: New topic
    description: A new help article or guide
  - label: Revision
    description: Updating an existing article
  - label: Release notes
    description: Announcing changes
```

**User sees:**
```
What type of document are you creating?
  ○ New topic - A new help article or guide
  ○ Revision - Updating an existing article
  ○ Release notes - Announcing changes
```

### Type 2: Multiple Select (Pick Many)

Use when: User can select multiple options (use `multiSelect: true`).

```yaml
header: features_included
question: What should this guide cover?
multiSelect: true
options:
  - label: Step-by-step instructions
  - label: Code examples
  - label: Troubleshooting section
  - label: Video walkthrough
  - label: FAQ
```

**User sees:**
```
What should this guide cover? (select all that apply)
  ☐ Step-by-step instructions
  ☐ Code examples
  ☐ Troubleshooting section
  ☐ Video walkthrough
  ☐ FAQ
```

### Type 3: Free Text Input

Use when: User can enter any text response (no options = free text).

```yaml
header: target_audience
question: Who is the primary audience for this doc?
```

**User sees:**
```
Who is the primary audience for this doc?
[___________________] (free text field)
```

### Type 4: Yes/No

Use when: Simple yes/no decision needed.

```yaml
header: include_examples
question: Should we include code examples?
options:
  - label: Yes
  - label: No
```

## Question Sequencing

### Bad UX ❌
```
Ask 10 questions all at once
→ User overwhelmed
→ Answers may be inconsistent
```

### Good UX ✅
```
Ask 1-3 critical questions
→ Get answers
→ Ask follow-up questions based on answers
→ Prevent question overload
```

## Standard Question Flows

### Flow 1: New Documentation

```
Q1: What type of document?
  ↓
  Options: new-topic, revision, release-notes, troubleshooting, process-doc
  ↓
Q2: Who is the audience?
  ↓
  Options: end-users, developers, team, everyone
  ↓
Q3: What tone?
  ↓
  Options: casual, professional, technical, friendly
  ↓
Q4: Special requirements?
  ↓
  Options: screenshots, code examples, diagrams, video
```

### Flow 2: Revision

```
Q1: What's being updated?
  ↓
  Free text: [existing-file.md]
  ↓
Q2: Why is it changing?
  ↓
  Options: UI changed, feature updated, new use case, fix errors
  ↓
Q3: Scope of changes?
  ↓
  Options: minor-update, major-rewrite, consolidate-docs
```

### Flow 3: Release Notes

```
Q1: What version?
  ↓
  Free text: [v2.5.0]
  ↓
Q2: What changed?
  ↓
  Multiple select: breaking-changes, new-features, bug-fixes, security-fixes
  ↓
Q3: Known issues?
  ↓
  Free text or no
```

## Question Template

Use this template for all questions:

```yaml
questions:
  - header: question_id_1
    question: "Short, clear question (10-15 words max)"
    options:
      - label: Option 1
        description: Brief description of when to use
        recommended: true  # Optional: highlight best choice
      - label: Option 2
        description: Brief description

  - header: question_id_2
    question: "Next question"
    multiSelect: false
    options:
      - label: Choice A
      - label: Choice B
```

## Implementation Example

### Agent Code

```python
from vscode_askQuestions import ask

# Ask initial questions
responses = ask(
  questions=[
    {
      "header": "doc_type",
      "question": "What are you documenting?",
      "options": [
        {"label": "New feature"},
        {"label": "Existing feature update"},
        {"label": "Release announcement"},
      ]
    },
    {
      "header": "audience",
      "question": "Who is the primary audience?",
      "options": [
        {"label": "End users"},
        {"label": "Developers"},
        {"label": "Team members"},
      ]
    }
  ]
)

# Use responses
doc_type = responses["doc_type"]
audience = responses["audience"]

if doc_type == "New feature":
    template = get_new_doc_template(audience)
elif doc_type == "Existing feature update":
    template = get_revision_template(audience)
else:
    template = get_release_notes_template()

return template
```

## Decision Logic Based on Answers

```
IF doc_type == "new-topic" AND audience == "developers":
    TEMPLATE: technical-guide-template.md
    INCLUDE: Code examples, API reference
    TONE: Technical but accessible

IF doc_type == "revision" AND scope == "major-rewrite":
    CONFIRM: User approval needed before rewriting
    ACTION: Show outline first

IF doc_type == "release-notes" AND breaking_changes == true:
    INCLUDE: Migration guide section
    HIGHLIGHT: "Breaking Changes" section first
```

## Follow-up Questions

After initial response, ask targeted follow-ups:

```
User selects: "New topic"

Q1 (done) - What type? → "New topic"

Q2 - What's the feature?
    Free text: [feature name]

Q3 - Do you have a GitHub issue or Figma design?
    Options: GitHub issue, Figma design, Both, Neither

IF "Both":
    Q4 - Can you share the links?
    Free text: [issue #123 / design URL]
```

## Common Question Patterns

### Pattern 1: Disambiguate Intent

```yaml
question: Is this updating an existing doc or creating a new one?
options:
  - label: New documentation
  - label: Update existing
  - label: Not sure (show me options)
```

### Pattern 2: Confirm Audience

```yaml
question: Who will primarily use this documentation?
options:
  - label: End users / customers
    description: Non-technical, task-focused
  - label: Developers / technical users
    description: Technical, implementation-focused
  - label: Both / mixed audience
    description: Balance technical and non-technical
```

### Pattern 3: Gather Context Source

```yaml
question: What's your primary information source?
options:
  - label: GitHub issue
  - label: Figma design
  - label: Jira ticket
  - label: Customer request
  - label: Team discussion
  - label: Other / combination
```

### Pattern 4: Scope Confirmation

```yaml
question: How much content?
options:
  - label: Quick guide (< 500 words)
  - label: Standard guide (500-2000 words)
  - label: Comprehensive guide (2000+ words)
  - label: Multi-part series
```

### Pattern 5: Special Requirements

```yaml
question: What should we include?
multiSelect: true
options:
  - label: Step-by-step instructions
  - label: Code examples
  - label: Screenshots / diagrams
  - label: Troubleshooting section
  - label: Related reading / links
  - label: FAQ
```

## Best Practices

### ✅ Do This

- Keep questions simple and direct
- Use clear, jargon-free language
- Provide helpful descriptions for each option
- Ask 1-3 questions per "round"
- Wait for answers before asking follow-ups
- Use `recommended: true` to highlight best choice

### ❌ Don't Do This

- Ask 10+ questions at once
- Use vague or ambiguous wording
- Make questions too technical
- Force answers when user can explain freely
- Ask questions user already answered
- Force user through all questions if early answers clarify intent

## Error Handling

What if user doesn't answer?

```
IF user skips question or provides unclear answer:
    SHOW: "I didn't quite understand. Could you clarify?"
    ALLOW: Free text input
    OR: Provide examples
```

## Customization

1. **Define your question categories** - What matters for your docs?
2. **Create answer decision tree** - What happens after each answer?
3. **Adjust audience options** - Who uses your docs?
4. **Add custom requirements** - What should every doc have?

## Related Skills

- **determine-content-scope** - Use questions to clarify document type
- **content-audit** - Ask if user wants to update existing docs
- **github-aware-writing** - Ask for GitHub issue/PR context
