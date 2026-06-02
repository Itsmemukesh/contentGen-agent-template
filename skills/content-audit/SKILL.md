# Skill: Content Audit

## Purpose

Search the documentation repository to determine if content already exists, preventing duplication and identifying where new content should fit.

## What This Skill Does

- **Searches existing docs** for related content
- **Identifies duplicates** - Is this already documented elsewhere?
- **Finds best location** - Where should this content live?
- **Suggests consolidation** - Should we merge similar docs?
- **Maps relationships** - How does this fit with existing topics?
- **Prevents gaps** - What related topics should we cross-reference?

## Audit Process

### Step 1: Search for Existing Content

Search the repo for:
- Topic title keywords
- Feature names
- Problem statements
- User workflow keywords

**Example:**
```
Searching for: "two-factor authentication"

Results:
  1. /guides/security/2fa-setup.md ← Exact match!
  2. /guides/account-settings.md (has 2FA section)
  3. /reference/api-auth.md (mentions 2FA)
  4. /troubleshooting/login-issues.md (related)
```

### Step 2: Analyze Existing Content

For each result, check:
- ✅ **Exact match** - Identical topic already exists
- ⚠️ **Partial match** - Related content exists
- 🔗 **Cross-reference** - Should link to/from this
- ❌ **No match** - Content is new

### Step 3: Make Recommendation

**If exact match found:**
```
Recommendation: REVISION
  - Topic exists at: /guides/security/2fa-setup.md
  - Current content: [summary]
  - Update needed: [what changed]
```

**If partial match found:**
```
Recommendation: EXTEND or CONSOLIDATE
  - Related content at: /guides/account-settings.md
  - Suggestion 1: Add new section to existing doc
  - Suggestion 2: Create standalone doc with cross-references
```

**If no match:**
```
Recommendation: CREATE NEW
  - Best location: /guides/security/
  - Similar docs here: [list for reference]
  - Cross-reference with: [related topics]
```

## Search Strategies

### Strategy 1: Keyword Search

Search for topic keywords in filenames and content:

```
Keywords: ["two-factor", "2fa", "authentication", "mfa"]

Search in:
  - Filenames (guides, tutorials, references)
  - Headings (h1, h2, h3)
  - Content (paragraphs, sections)
  - Metadata (tags, labels)
```

### Strategy 2: Document Type Search

Search by doc type and location:

```
Looking for: New "troubleshooting guide"

Search in:
  - /troubleshooting/ folder
  - Docs with "troubleshooting" or "FAQ" in title
  - Related issue tracker labels
```

### Strategy 3: Related Topic Search

Find documents linked to/from the topic:

```
Current topic: "Getting Started"

Find:
  - Links TO "Getting Started" (what references it?)
  - Links FROM "Getting Started" (what does it reference?)
  - Same folder / category (sibling topics)
```

### Strategy 4: Metadata Search

Search by tags, categories, or labels:

```
Tags/Labels: ["security", "authentication", "enterprise"]

Find all docs tagged with these, even if keywords don't match exactly.
```

## Implementation Examples

### Example 1: Exact Match - Revision Needed

```
User request: "Document the new 2FA flow"

Search:
  Query: "two-factor authentication"
  Location: /guides/

Result:
  ✅ Found: /guides/security/2fa-setup.md

Analysis:
  - Content exists: "Setup 2FA"
  - Last updated: 3 months ago
  - Contains sections: Setup, Recovery codes, Troubleshooting
  - PROBLEM: Missing "new flow" details

Recommendation:
  📝 ACTION: REVISION
  📍 File: /guides/security/2fa-setup.md
  🔄 Update: Add section: "2FA Flow (Updated)" with new screenshots
  🔗 Cite: GitHub PR #789 with changes
```

### Example 2: Partial Match - Extend Existing

```
User request: "Document team 2FA requirements"

Search:
  Query: "team authentication" OR "team security"
  Location: /guides/

Results:
  1. /guides/security/2fa-setup.md (personal 2FA)
  2. /guides/team-management/team-settings.md (team admin guide)
  3. /guides/team-management/roles-permissions.md (RBAC)

Analysis:
  - No dedicated "team 2FA" doc exists
  - Team settings doc covers other team features
  - Good opportunity to extend team-settings.md

Recommendation:
  🔄 ACTION: EXTEND or CREATE
  📍 Option 1: Add section to /guides/team-management/team-settings.md
  📍 Option 2: Create new /guides/security/team-2fa-requirements.md
  
  If Option 1:
    - Add subsection: "Requiring 2FA for team members"
    - Link to personal 2FA setup guide
    - Include team admin instructions
  
  If Option 2:
    - Create standalone guide
    - Cross-reference from team-settings.md
```

### Example 3: No Match - Create New

```
User request: "Document API rate limiting"

Search:
  Query: "rate limiting" OR "rate limit" OR "throttling"
  Location: /reference/ and /api/

Result:
  ❌ No results found

Analysis:
  - Content is completely new
  - Related docs exist: /api/authentication.md, /api/errors.md
  - Logical location: /api/ folder

Recommendation:
  ✨ ACTION: CREATE NEW
  📍 Location: /api/rate-limiting.md
  🔗 Cross-reference from:
    - /api/errors.md (429 Too Many Requests)
    - /reference/api-overview.md (add to "API Concepts" section)
  📚 Similar structure to: /api/authentication.md
```

### Example 4: Consolidation Opportunity

```
User request: "FAQ about authentication"

Search:
  Query: "authentication" AND ("question" OR "faq" OR "troubleshoot")
  
Results:
  1. /faq/account-faq.md (has 3 auth questions)
  2. /troubleshooting/login-issues.md (auth troubleshooting)
  3. /guides/authentication.md (setup guide)

Analysis:
  - Auth content scattered across 3 docs
  - Overlap: Some content repeated
  - Opportunity: Consolidate into one "Authentication Guide"

Recommendation:
  🔄 ACTION: CONSIDER CONSOLIDATION
  📍 Option 1: Create /guides/authentication-complete.md with:
    - Setup (from /guides/authentication.md)
    - FAQ (from /faq/account-faq.md)
    - Troubleshooting (from /troubleshooting/login-issues.md)
    - Update other docs with links to consolidated guide
  
  📍 Option 2: Keep separate, add better cross-references
  
  ⚠️ NOTE: Consolidation is large scope—get user approval first
```

## Audit Checklist

Use this before starting new documentation:

```
□ Searched for keywords in existing docs
□ Searched in relevant folders
□ Checked file names and headings
□ Looked for related/similar topics
□ Reviewed document metadata/tags
□ Asked: Does this exact topic exist?
□ Asked: Is there related content?
□ Asked: Should we consolidate?
□ Documented: Search results and recommendation
□ Confirmed: User agreement with recommendation
```

## Agent Logic

```
FUNCTION audit_content(search_keywords, doc_type):
  
  # Search existing content
  results = search_repo(keywords=search_keywords, type=doc_type)
  
  IF results.count == 0:
    recommendation = "CREATE_NEW"
    best_location = infer_folder_location(doc_type)
    
  ELSE IF results.count == 1 AND exact_match(results[0]):
    recommendation = "REVISION"
    target_file = results[0].path
    
  ELSE IF results.count > 1:
    recommendation = "EXTEND_OR_CONSOLIDATE"
    candidates = rank_by_relevance(results)
    target_files = candidates[0:3]
    
  RETURN {
    recommendation: recommendation,
    target_file: target_file,
    related_files: related_files,
    folder_suggestion: folder_suggestion
  }
```

## Search Query Examples

### Query 1: Topic-Based
```
Keywords: "authentication", "two-factor", "2fa", "mfa", "otp"
File patterns: *.md in /guides/, /reference/, /api/
```

### Query 2: Problem-Based
```
Keywords: "can't log in", "login fails", "unauthorized", "access denied"
File patterns: *.md in /troubleshooting/
```

### Query 3: Workflow-Based
```
Keywords: "setup", "getting started", "first time", "onboarding"
File patterns: *.md in /guides/ with "setup" or "getting-started" in title
```

### Query 4: API-Based
```
Keywords: "POST /api/auth", "authentication endpoint", "login endpoint"
File patterns: *.md in /api/ or /reference/
```

## Folder Structure Template

Knowing your repo structure helps with audit:

```
/docs/
  /guides/              ← User workflows, how-to guides
  /reference/           ← API docs, specifications
  /troubleshooting/     ← Problem solving
  /tutorials/           ← Learning paths
  /faq/                 ← Questions and answers
  /release-notes/       ← Change announcements
  /architecture/        ← System design docs
```

When auditing, search in relevant folders first:
- New feature? → Search `/guides/` first
- API change? → Search `/reference/` and `/api/`
- User problem? → Search `/troubleshooting/`
- Release? → Search `/release-notes/`

## Customization

1. **Define your folder structure** - Where do docs live?
2. **Set search scope** - Which folders to search?
3. **Add custom tags/categories** - How do you organize?
4. **Define consolidation rules** - When to merge docs?
5. **Create folder templates** - What goes in each folder?

## Related Skills

- **determine-content-scope** - Know doc type before auditing
- **markdown-validation** - Validate found content quality
- **github-aware-writing** - Cross-reference GitHub issues
