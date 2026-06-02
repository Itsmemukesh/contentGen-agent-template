# Example: Creating a New Topic

## Scenario

Your team released a new authentication feature (two-factor authentication). You need to document it.

**Context:**
- GitHub issue: #456 - "Add two-factor authentication"
- Figma design: [link to design]
- Feature status: Released in v2.5

## Agent Workflow

### Step 1: User Request

```
User: "Create a guide for our new two-factor authentication feature. 
We have GitHub issue #456 and a Figma design."
```

### Step 2: Agent Clarification (Questions)

Agent asks:

```
Q1: What type of document?
    Options: New topic, Revision, Release notes, Troubleshooting, Process doc
    → User selects: "New topic"

Q2: Who is the primary audience?
    Options: End users, Developers, Team members, Everyone
    → User selects: "End users"

Q3: What tone?
    Options: Casual, Professional, Technical, Friendly
    → User selects: "Professional but friendly"

Q4: What should we include?
    Options: Step-by-step, Code examples, Screenshots, Troubleshooting, FAQ
    → User selects: "Step-by-step, Screenshots, Troubleshooting"
```

### Step 3: Content Audit

Agent searches existing docs:

```
Searching for: "two-factor", "2fa", "authentication"

Results:
  - /guides/security/account-security.md (mentions auth)
  - /reference/api-auth.md (API reference only)

Recommendation:
  ✅ No exact match - this is new content
  📍 Best location: /guides/security/two-factor-authentication.md
  🔗 Cross-reference from: /guides/security/account-security.md
```

### Step 4: Context Gathering

Agent fetches from GitHub and Figma:

```
From GitHub issue #456:
  - Title: Add two-factor authentication
  - Description: Support TOTP apps and email codes
  - Implementation: Users can enable via Account Settings
  - Benefits: Improved security, enterprise compliance

From Figma design:
  - Screenshots: Setup flow, enable screen, recovery codes
  - UI labels: "Add Two-Factor Authentication", "Choose Method", "Verify Code"
  - User paths: Personal 2FA, Team 2FA requirements
```

### Step 5: Propose Outline

Agent shows outline for approval:

```
Proposed structure:

# Two-Factor Authentication

## Overview
Brief intro explaining what 2FA is and why it matters

## Supported Methods
- Authenticator apps (TOTP)
- Email codes

## Enabling 2FA
1. Go to Account Settings
2. Select Security
3. Click "Add Two-Factor Authentication"
4. Choose method
5. Verify code

## Managing 2FA
- Recovery codes
- Disabling 2FA
- Losing your device

## Troubleshooting
- Codes not working
- Lost authenticator device
- Recovery codes exhausted

Approve? → (waits for user feedback)
```

### Step 6: Write Content

User approves, agent writes Markdown:

```markdown
# Two-Factor Authentication

Two-factor authentication (2FA) adds an extra layer of security to your account. 
After you enter your password, you'll need to enter a code from your authenticator 
app or email to complete login. This prevents unauthorized access even if someone 
has your password.

## Supported Methods

We support two methods of two-factor authentication:

### Authenticator Apps (TOTP)
Time-based one-time passwords (TOTP) work offline on your mobile device. 
We recommend this method for most users.

Supported apps:
- Google Authenticator
- Microsoft Authenticator
- Authy

### Email Codes
We'll send a verification code to your registered email address after you enter 
your password.

## Enabling Two-Factor Authentication

1. Log in to your account
2. Go to **Settings** → **Security**
3. Click **Add Two-Factor Authentication**
4. Choose your preferred method:
   - Authenticator app: Scan the QR code with your app
   - Email: We'll send codes to your registered email
5. Enter the verification code to confirm
6. Save your recovery codes in a safe place

## Managing Your 2FA

### Recovery Codes
Save your recovery codes when you set up 2FA. Use them if you:
- Lose access to your authenticator app
- Don't receive an email code
- Can't access your phone

Store recovery codes safely. Never share them.

### Changing Your Method
You can switch between email and authenticator app anytime in **Security Settings**.

### Disabling 2FA
You can turn off 2FA in **Security Settings**. Your account will be less secure.

## Troubleshooting

### My authenticator code doesn't work
1. Check your device time is correct (codes depend on exact time)
2. Ensure you're using the code immediately (codes change every 30 seconds)
3. Try a recovery code instead
4. Contact support if problems persist

### I lost my authenticator device
Use a recovery code to log in, then:
1. Go to **Security Settings**
2. Click **Manage Two-Factor**
3. Choose a new method and verify

## Need Help?
Contact support or see [related articles](link-to-other-guides).
```

### Step 7: Validate

Agent validates:

```
✅ Markdown syntax valid
✅ All headings properly formatted
✅ No broken links
✅ File name: two-factor-authentication.md (kebab-case)
✅ Required sections present:
   - Overview ✓
   - Methods ✓
   - Setup steps ✓
   - Management ✓
   - Troubleshooting ✓
✅ Cites GitHub issue #456
```

### Step 8: Summary

Agent reports completion:

```
✅ CREATED: /guides/security/two-factor-authentication.md

📊 Content:
   - Sections: 6 (Overview, Methods, Setup, Management, Troubleshooting, Help)
   - Word count: ~800 words
   - Includes: Steps, screenshots, recovery instructions

📝 Sources:
   - GitHub issue #456
   - Figma design screenshots
   - Feature spec

🔗 Cross-referenced from:
   - /guides/security/account-security.md (add link)
   - /release-notes/v2.5.md (add to "New Features")

✨ Ready to publish!
```

## Result

New documentation file created and ready to publish. User can copy to their docs system, review, and merge.

## Next Steps (Manual)

1. Review content for accuracy
2. Add screenshots from Figma design
3. Create cross-reference links
4. Publish to documentation site
5. Add to table of contents
