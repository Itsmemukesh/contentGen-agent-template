# Skill: GitHub-Aware Writing

## Purpose

Ensure documentation is written with awareness of GitHub context: issues, PRs, commits, discussions. Extract relevant information from GitHub and incorporate it naturally into docs.

## What This Skill Does

- **Extracts context** from GitHub issues, PRs, discussions
- **Recognizes** issue numbers, PR references, commit hashes
- **Links appropriately** to GitHub resources
- **Cites sources** - Shows where information came from
- **Incorporates examples** from code in PRs
- **Updates docs** based on merged changes
- **Maintains accuracy** by tying docs to actual implementation

## GitHub Context Sources

### 1. GitHub Issues

**Extract from issue:**
- Title → Main topic/heading
- Body → Key requirements, acceptance criteria
- Labels → Document type, priority
- Assignees → Subject matter experts
- Comments → Discussion, edge cases, clarifications

**Example:**
```
Issue: #456 - Add two-factor authentication
Description: Users should be able to enable 2FA via email or authenticator app
Labels: feature, security
Assignee: @security-team

→ Use this as source for: guides/two-factor-authentication.md
→ Include: Requirements, use cases, trade-offs from discussion
```

### 2. Pull Requests

**Extract from PR:**
- Title → What changed
- Description → Why it changed
- Files changed → What was added/modified/removed
- Commits → Implementation details
- Reviews → Important considerations

**Example:**
```
PR: #789 - Refactor auth module
Changes: Updated login flow, deprecated old API
Files: src/auth.js, tests/auth.test.js

→ Use this as source for: revision of authentication guide
→ Include: What's new, migration guide, examples of new API
```

### 3. Discussions & Comments

**Extract from discussions:**
- Q&A threads → FAQ content
- Troubleshooting threads → Troubleshooting guide content
- Implementation discussions → Edge cases, rationale
- User feedback → Real-world use cases

**Example:**
```
Discussion: "How do I configure 2FA for my team?"
→ Use for troubleshooting-two-factor-authentication.md
→ Include: Common setup issues, team-level configuration
```

## Linking to GitHub Resources

### Issue Links
```markdown
[GitHub issue #456](https://github.com/your-org/your-repo/issues/456)
```

### Pull Request Links
```markdown
[GitHub PR #789](https://github.com/your-org/your-repo/pull/789)
```

### Commit Links
```markdown
[Commit abc1234](https://github.com/your-org/your-repo/commit/abc1234)
```

### Discussion Links
```markdown
[GitHub discussion](https://github.com/your-org/your-repo/discussions/12)
```

### File Links
```markdown
[See implementation](https://github.com/your-org/your-repo/blob/main/src/auth.js)
```

## Citing Sources in Docs

When pulling information from GitHub, cite the source:

### Option 1: Inline Citation
```markdown
The 2FA feature (see [GitHub issue #456](https://github.com/your-org/your-repo/issues/456)) 
allows users to enable authentication via email or authenticator apps.
```

### Option 2: Source Block
```markdown
> Source: [GitHub issue #456](https://github.com/your-org/your-repo/issues/456)
> 
> This feature was designed to improve security for enterprise users.
```

### Option 3: Related Issues Section
```markdown
## Related Issues

- [Add two-factor authentication #456](https://github.com/your-org/your-repo/issues/456)
- [2FA for teams #512](https://github.com/your-org/your-repo/issues/512)
```

## Incorporating GitHub Context

### From Issues

```markdown
❌ Bad - No context:
# Two-Factor Authentication
Enable 2FA to secure your account.

✅ Good - With context:
# Two-Factor Authentication

Enable two-factor authentication (2FA) to add an extra layer of security 
to your account. Our 2FA implementation supports both email and 
authenticator apps (see [GitHub issue #456](https://github.com/your-org/your-repo/issues/456)).

## Why Use 2FA?
- Prevents unauthorized access
- Protects sensitive operations
- Meets enterprise security requirements
```

### From Pull Requests

```markdown
❌ Bad - No mention of changes:
# API Reference
Use the authentication API to authenticate users.

✅ Good - Explains what changed:
# API Reference

## Authentication Endpoint (Updated in [PR #789](https://github.com/your-org/your-repo/pull/789))

We've refactored the authentication module to improve security and performance.

### New Endpoint
\`\`\`bash
POST /api/v2/auth/login
\`\`\`

### Migration from Old API
If you're using the deprecated `/api/v1/auth/login`, please migrate to the new endpoint.
See [migration guide](#migration) below.
```

### From Discussions

```markdown
❌ Bad - No real-world examples:
# Troubleshooting 2FA
If 2FA doesn't work, try these steps.

✅ Good - Addresses real user questions:
# Troubleshooting 2FA

## "I set up 2FA but can't log in with my authenticator app"

This is a common issue (see [GitHub discussion](https://github.com/your-org/your-repo/discussions/42)).

**Solution:**
1. Ensure your device clock is synchronized (authenticator apps use time-based codes)
2. Check that you're using the correct code (codes change every 30 seconds)
3. Try resetting your 2FA...
```

## Agent Logic

```
FUNCTION write_with_github_context(issue_number, pr_number):
  
  # Fetch GitHub context
  IF issue_number:
    issue = github.get_issue(issue_number)
    context = {
      title: issue.title,
      body: issue.body,
      labels: issue.labels,
      discussion: issue.comments
    }
  
  IF pr_number:
    pr = github.get_pr(pr_number)
    context = {
      title: pr.title,
      changes: pr.files_changed,
      commits: pr.commits,
      reviews: pr.reviews
    }
  
  # Incorporate into doc
  doc_section = extract_main_idea(context)
  doc_section += cite_source(github_url)
  doc_section += include_key_details(context)
  
  # Add examples
  IF pr_number:
    code_examples = extract_code(pr.changed_files)
    doc_section += format_examples(code_examples)
  
  RETURN doc_section
```

## Examples

### Example 1: New Feature from Issue

**GitHub Issue #456:**
- Title: Add two-factor authentication
- Body: Support TOTP apps and email codes
- Labels: feature, security

**Generated Doc:**
```markdown
# Two-Factor Authentication

Enable two-factor authentication (2FA) to add extra security to your account.

See [GitHub issue #456](https://github.com/your-org/repo/issues/456) for feature details.

## Supported Methods

- **Authenticator apps** (TOTP) - Time-based one-time passwords
- **Email codes** - Recovery codes sent to registered email

## How to Enable 2FA

1. Go to Account Settings → Security
2. Click "Enable Two-Factor Authentication"
3. Choose your preferred method
4. Follow the setup instructions
```

### Example 2: Updated Feature from PR

**GitHub PR #789:**
- Title: Refactor authentication API
- Files: src/auth.js, tests/auth.test.js
- Changes: New v2 endpoint, deprecate old v1

**Generated Doc:**
```markdown
# API Reference - Authentication (Updated)

Updated in [PR #789](https://github.com/your-org/repo/pull/789).

## Login Endpoint

\`\`\`bash
POST /api/v2/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
\`\`\`

### Migrating from v1

The old `/api/v1/auth/login` endpoint is deprecated. Please migrate to v2.

**Changes:**
- Response format updated
- Error codes refined
- Rate limiting improved
```

### Example 3: FAQ from Discussion

**GitHub Discussion #42:**
- Q: "How do I reset 2FA if I lost my phone?"
- Q: "Can we use 2FA for team accounts?"
- Q: "Does 2FA work offline?"

**Generated Doc:**
```markdown
# Two-Factor Authentication FAQ

Based on [community discussions](https://github.com/your-org/repo/discussions).

## Q: How do I reset 2FA if I lost my phone?

A: Contact our support team with proof of identity. We'll help you regain access.

## Q: Can we enable 2FA for team accounts?

A: Yes! Team admins can require 2FA for all team members.
See [GitHub discussion #512](https://github.com/your-org/repo/discussions/512).

## Q: Does 2FA work offline?

A: Authenticator apps work offline. Email codes require internet connection.
```

## Best Practices

1. **Always cite your source** - Users want to know where info comes from
2. **Link to issues/PRs** - Let users see the full context
3. **Keep docs in sync with implementation** - Update docs when code changes
4. **Extract examples from code** - Show real implementation, not imaginary
5. **Use issue discussions** - FAQ/troubleshooting benefit from real user questions
6. **Version your docs** - Link to specific commits/tags for reference

## Customization

1. **Add your GitHub org URL** - Replace `your-org/your-repo` throughout
2. **Adjust citation style** - Use inline or block format based on preference
3. **Define which GitHub info to include** - Issues? PRs? Discussions? All?
4. **Set guidelines for when to cite** - Always? Only for major changes?

## Related Skills

- **markdown-validation** - Ensure links to GitHub are valid
- **github-aware-writing** - This skill!
- **content-audit** - Check if related GitHub issues/PRs exist
