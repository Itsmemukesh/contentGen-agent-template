# Example: Creating Release Notes

## Scenario

Your team just released v2.5.0 with new features and fixes. You need to create release notes for the announcement.

**Context:**
- Version: 2.5.0
- Release date: Today
- Features: 2 new, 5 fixes, 1 breaking change
- GitHub: Merged PRs show what changed
- Jira: Tickets map features to work

## Agent Workflow

### Step 1: User Request

```
User: "Create release notes for v2.5.0. We have 2 new features, 5 bug fixes, 
and one breaking change we need to highlight."
```

### Step 2: Agent Clarification

Agent asks:

```
Q1: Document type?
    → User selects: "Release notes"

Q2: What should we include?
    Multi-select options:
      ☑ New Features
      ☑ Bug Fixes
      ☑ Breaking Changes
      ☑ Known Issues
      ☑ Migration Guide
    → User selects all

Q3: Should we highlight specific items?
    Free text: → User enters: "Two-factor auth is major"

Q4: Do you have a GitHub milestone or PR list?
    Free text: → User enters: "Milestone: v2.5.0, PRs: #750, #761, #789"
```

### Step 3: Context Gathering

Agent fetches GitHub PRs and Jira tickets:

```
From GitHub (Milestone v2.5.0):
  PR #750: Add two-factor authentication (Feature)
  PR #761: Improve password recovery (Feature)
  PR #781: Fix login timeout issue (Fix)
  PR #784: Fix email validation bug (Fix)
  PR #789: Refactor auth module (Infrastructure)
  PR #792: Fix UI rendering on mobile (Fix)
  PR #801: Update API error messages (Fix)
  PR #809: Deprecate old auth endpoint (Breaking Change)

From Jira:
  [FEAT-456] Two-factor authentication
    - Status: Completed
    - Impact: Security enhancement
  
  [FEAT-512] Enhanced password recovery
    - Status: Completed
    - Impact: User experience
  
  [BREAK-789] Deprecate /api/v1/auth/login
    - Replacement: /api/v2/auth/login
    - Migration deadline: 60 days
```

### Step 4: Organize Changes

Agent categorizes and prioritizes:

```
CATEGORY: New Features (2)
  1. ⭐ Two-Factor Authentication (PR #750, FEAT-456)
     - TOTP apps and email codes supported
     - Enterprise security compliance
     - Priority: HIGH (user-facing)
  
  2. Enhanced Password Recovery (PR #761, FEAT-512)
     - Faster recovery flow
     - Email recovery codes
     - Priority: MEDIUM

CATEGORY: Bug Fixes (4)
  1. Fixed login timeout when idle (PR #781)
  2. Fixed email validation edge cases (PR #784)
  3. Fixed mobile UI rendering (PR #792)
  4. Updated API error messages for clarity (PR #801)

CATEGORY: Breaking Changes (1)
  - ⚠️ Deprecated /api/v1/auth/login endpoint (PR #809)
    - Replacement: /api/v2/auth/login
    - Sunset date: 60 days
    - Priority: HIGH (developer-facing)

CATEGORY: Infrastructure
  - Auth module refactored (PR #789) - not user-facing
```

### Step 5: Propose Structure

Agent shows proposed release notes outline:

```
Proposed v2.5.0 Release Notes:

# Release Notes: v2.5.0

**Released:** [Date]

## ✨ What's New

### Two-Factor Authentication
Enable extra security with TOTP apps or email codes.
See: [Setup guide](../guides/security/2fa.md)

### Enhanced Password Recovery
Faster, more secure password recovery process.
See: [Updated guide](../guides/account-settings/password-recovery.md)

## 🐛 Bug Fixes

- Fixed login timeout when idle (PR #781)
- Fixed email validation for edge cases (PR #784)
- Fixed mobile UI rendering issues (PR #792)
- Improved API error messages for clarity (PR #801)

## ⚠️ Breaking Changes

### API Endpoint Deprecation
The `/api/v1/auth/login` endpoint is deprecated.

**Migrate to:** `/api/v2/auth/login`
**Deadline:** 60 days (by [date])
**Migration guide:** [See docs](../api/migration-guide-v1-to-v2.md)

## 📋 Known Issues

- Two-factor timeout may occur in offline scenarios
- Email recovery codes take up to 5 minutes to arrive

## 🙏 Thank You
Thanks to all contributors who made v2.5.0 possible!

Approve? (waits for user)
```

### Step 6: Write Release Notes

User approves, agent creates Markdown:

```markdown
# Release Notes: v2.5.0

**Released:** February 15, 2025

Welcome to v2.5.0! This release brings enhanced security features and significant 
improvements to the authentication experience.

## ✨ What's New

### Two-Factor Authentication (2FA)

We've added two-factor authentication to secure your account beyond just a password.

**Features:**
- **Authenticator apps** (TOTP) - Works offline, recommended for most users
- **Email codes** - Recovery codes sent to your registered email
- **Recovery codes** - Safe-keep codes for account recovery

Enable 2FA in Account Settings → Security → Add Two-Factor Authentication.

**Learn more:** [Two-Factor Authentication Guide](link)

**GitHub:** [PR #750](https://github.com/your-org/repo/pull/750)

### Enhanced Password Recovery

Password recovery is now faster and more secure.

**Improvements:**
- Recovery links valid for 2 hours (was 30 minutes)
- Multiple recovery code options
- Clearer instructions and confirmation

**Learn more:** [Password Recovery Guide](link)

**GitHub:** [PR #761](https://github.com/your-org/repo/pull/761)

## 🐛 Bug Fixes

This release includes 4 important bug fixes:

| Issue | Fix | PR |
|-------|-----|-----|
| Login timeout when idle | Session now persists correctly | [#781](link) |
| Email validation edge cases | Fixed acceptance of invalid emails | [#784](link) |
| Mobile UI rendering | Fixed button alignment on small screens | [#792](link) |
| Unclear API errors | Improved error messages for developers | [#801](link) |

## ⚠️ Breaking Changes

### API Endpoint Deprecation

The `/api/v1/auth/login` endpoint is now deprecated and will be removed in 60 days.

**What changed:**
- Old endpoint: `POST /api/v1/auth/login`
- New endpoint: `POST /api/v2/auth/login`
- Response format updated
- Error codes refined

**Action required:** Update your API clients to use the v2 endpoint.

**Migration deadline:** April 15, 2025

**See:** [API Migration Guide](link)

**GitHub:** [PR #809](https://github.com/your-org/repo/pull/809)

## 📋 Known Issues

- **2FA timeout in offline mode** - Two-factor verification may timeout if device loses internet during code generation. Use recovery codes as alternative.
- **Email recovery delays** - Recovery emails may take up to 5 minutes to arrive. Check spam folder if not received.

We're investigating both issues and will provide fixes in v2.5.1.

## 📊 Statistics

- **Features:** 2
- **Bug fixes:** 4
- **Breaking changes:** 1
- **PRs merged:** 9
- **Commits:** 47
- **Lines changed:** 2,847

## 🙏 Thank You

Special thanks to the engineering team for shipping these improvements. 
Thanks to beta testers for catching critical edge cases.

---

**Have questions?** 
- [View documentation](link)
- [Open an issue](link)
- [Contact support](link)

**Upgrade now:** [Download v2.5.0](link)
```

### Step 7: Validate

Agent validates:

```
✅ Markdown syntax valid
✅ All PR links formatted correctly
✅ Breaking changes clearly highlighted
✅ Migration information provided
✅ Known issues documented
✅ Cites all source PRs and issues
✅ Professional tone
✅ User-facing vs. developer-facing clearly separated
```

### Step 8: Summary

Agent reports completion:

```
✅ CREATED: /release-notes/v2.5.0.md

📊 Content:
   - Sections: 6 (What's New, Bug Fixes, Breaking Changes, Known Issues, Stats, Thank You)
   - Features highlighted: 2
   - Bugs fixed: 4
   - Breaking changes: 1
   - Known issues: 2
   - PRs cited: 9

🔗 External sources:
   - GitHub PRs (#750, #761, #781, #784, #789, #792, #801, #809)
   - Jira tickets (FEAT-456, FEAT-512, BREAK-789)

📝 Includes:
   - Feature descriptions with links to guides
   - Clear migration path for breaking changes
   - Known issues and workarounds
   - Statistics and contributor thanks

✨ Ready to announce!
```

## Result

Release notes created and ready for announcement. User can review, add screenshots, and publish.

## Next Steps (Manual)

1. Add product screenshots/GIFs for new features
2. Verify all internal links work
3. Add version download links
4. Schedule social media announcements
5. Send email notification to users
6. Create in-app notification banner
7. Publish to website/documentation

---

## Additional Notes

This release is significant for security (2FA) and has breaking API changes. 
Recommend highlighting in all communication channels and giving enterprise customers 
extra time to migrate API usage.
