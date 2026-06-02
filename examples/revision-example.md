# Example: Revising Existing Documentation

## Scenario

The login UI just changed significantly. The existing guide `/guides/getting-started/login.md` has outdated screenshots and steps. You need to update it.

**Context:**
- Existing doc: `/guides/getting-started/login.md`
- What changed: GitHub PR #789 refactored login flow
- Impact: Screenshots and steps are now incorrect

## Agent Workflow

### Step 1: User Request

```
User: "The login UI just changed in PR #789. Can you update the login guide?"
```

### Step 2: Agent Clarification

Agent asks:

```
Q1: Document type?
    → User selects: "Revision"

Q2: What changed?
    Options: UI changed, Feature updated, New use case, Fix errors
    → User selects: "UI changed"

Q3: Scope of changes?
    Options: Minor update, Major rewrite, Consolidate docs
    → User selects: "Major rewrite"

Q4: Do you have a PR or GitHub issue?
    Free text: → User enters: "PR #789"
```

### Step 3: Content Audit

Agent finds existing document:

```
Searching for: "login" guide

Found:
  ✅ /guides/getting-started/login.md (EXACT MATCH)
     - Last updated: 3 months ago
     - Sections: Overview, Steps, Troubleshooting
     - Status: NEEDS UPDATE
```

### Step 4: Context Gathering

Agent fetches GitHub PR #789:

```
From GitHub PR #789:
  Title: Refactor authentication module
  Changes:
    - New login endpoint (/api/v2/auth/login)
    - UI redesigned with new form layout
    - Password recovery flow changed
    - Added "Remember me" option
    
  Files changed:
    - src/auth/login.js (new flow)
    - src/components/LoginForm.jsx (new UI)
    - tests/auth.test.js
    
  Screenshots:
    - Old login form
    - New login form
    
  Commits:
    - "Update login UI"
    - "Add remember me feature"
```

### Step 5: Compare Old vs. New

Agent analyzes differences:

```
OLD vs. NEW Comparison:

SECTION: Overview
  OLD: "Enter email and password to log in"
  NEW: "Enter email and password. Optionally check 'Remember Me' to stay logged in."
  ACTION: Update with remember me info

SECTION: Steps
  OLD:
    1. Go to login page
    2. Enter email
    3. Enter password
    4. Click Sign In
  
  NEW:
    1. Go to login page
    2. Enter email
    3. Enter password
    4. Optional: Check "Remember Me" (30-day session)
    5. Click Sign In
  
  ACTION: Add remember me step + update screenshots

SECTION: Troubleshooting
  OLD: Covers forgotten password (old flow)
  NEW: Password recovery moved, needs new steps
  ACTION: Update recovery flow instructions

NEW SECTION NEEDED:
  - Remember Me feature explanation
```

### Step 6: Propose Changes

Agent shows what will be updated:

```
Proposed changes to /guides/getting-started/login.md:

✏️ SECTIONS TO UPDATE:
  1. Overview
     - Add: Remember Me option explanation
  
  2. Step-by-Step Login
     - Update: New form screenshot
     - Add: Remember Me step
     - Keep: Basic structure

  3. Password Recovery
     - Replace: Old recovery flow with new PR #789 flow
     - Add: Links to account settings

  4. Troubleshooting
     - Add: "Remember Me expired" issues
     - Update: Links to new endpoints

⭐ CITE SOURCE:
  GitHub PR #789 - Refactor authentication module

🔗 CROSS-REFERENCE:
  - Link from: /guides/account-settings/remember-me.md
  - Update: /release-notes/v2.6.md with login changes

Proceed? (waits for approval)
```

### Step 7: Update Document

User approves, agent updates Markdown:

```markdown
[OLD Content below with UPDATES marked]

# Logging In

Updated in [PR #789](https://github.com/your-org/your-repo/pull/789)

## Overview

To access your account, log in with your email and password. 
You can optionally check "Remember Me" to keep your session active for 30 days
on your current device—no need to log in again for that period.

## Step-by-Step Login

1. Go to the [login page](https://app.example.com/login)
2. Enter your email address
3. Enter your password
4. **(Optional)** Check "Remember Me" if you're on a device you trust
5. Click **Sign In**

That's it! You're now logged in to your account.

### Remember Me

When you check "Remember Me," your session will last for 30 days. 
This is useful for personal devices. **Don't use this on shared computers.**

Read more: [Remember Me security](../account-settings/remember-me-settings.md)

## Logging Out

Your session will automatically expire after 30 days (or 24 hours if you don't check Remember Me).
To log out immediately, click your profile → **Log Out**.

## Password Recovery

Forgot your password? 
1. Click **"Can't log in?"** on the login page
2. Enter your email address
3. Check your email for a recovery link (valid for 2 hours)
4. Click the link and set a new password

Recovery links are sent to your registered email. 
Check your spam folder if you don't see it.

## Two-Factor Authentication

If you have [two-factor authentication](../security/two-factor-authentication.md) enabled,
you'll be asked for a code after entering your password.

## Troubleshooting

### "Incorrect email or password"
Double-check your email and password are correct. Both are case-sensitive for passwords.

### "Remember Me isn't working"
Remember Me sessions expire after 30 days. Log in again to reset the timer.

### "Can't receive password reset email"
1. Check your spam/junk folder
2. Verify the email address associated with your account
3. Try again—recovery emails may take a few minutes
4. [Contact support](#need-help) if problems persist

## Need Help?
[Contact our support team](../support.md)
```

### Step 8: Validate

Agent validates changes:

```
✅ Markdown syntax valid
✅ All links point to existing files
✅ External links formatted correctly
✅ Cites GitHub PR #789
✅ Screenshots updated (reference to new forms)
✅ No new sections break existing structure
✅ Tone matches original
✅ All steps still accurate with new flow
```

### Step 9: Summary

Agent reports completion:

```
✅ UPDATED: /guides/getting-started/login.md

📝 Changes made:
   - Updated "Overview" section with Remember Me info
   - Added "Remember Me" subsection
   - Updated "Password Recovery" to new PR #789 flow
   - Added troubleshooting for Remember Me
   - Updated screenshots reference
   - Added cross-links to related topics

📊 Impact:
   - 3 sections modified
   - 2 sections enhanced
   - 1 subsection added
   - 0 sections removed

🔗 Sources:
   - GitHub PR #789 (primary)
   - Figma design for new UI

⚠️ Manual steps needed:
   1. Replace screenshots with new UI from Figma
   2. Test all links work
   3. Review password recovery section accuracy
   4. Publish to staging for QA review

Ready to merge!
```

## Result

Updated documentation reflecting PR #789 changes. User can review, test links, update screenshots, and publish.

## Next Steps (Manual)

1. Download new screenshots from Figma design
2. Replace old login form screenshots
3. Test all links point to correct pages
4. Review password recovery steps with team
5. Publish update to documentation site
6. Update table of contents if needed
7. Notify users of updated guide
