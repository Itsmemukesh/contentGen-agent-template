# Skill: Markdown Validation

## Purpose

Ensure generated Markdown meets quality standards: correct formatting, valid links, proper syntax, consistency with repo conventions.

## What This Skill Does

Validates:
- ✅ **Syntax** - Valid Markdown, no syntax errors
- ✅ **Links** - Internal links point to existing files, external links are valid
- ✅ **Formatting** - Headings, lists, code blocks, emphasis formatted correctly
- ✅ **Conventions** - Follows repo style (file naming, heading structure, line length)
- ✅ **Consistency** - Matches tone, terminology, formatting of existing docs
- ✅ **Structure** - Has required sections (overview, examples, etc.)

## Validation Rules (Customize These)

### 1. Markdown Syntax

**Check:**
- Headings use `#` syntax (not underlines)
- Lists use `-` or `*` consistently
- Code blocks use triple backticks with language
- Bold/italic use `**text**` and `*text*` (not HTML)
- No trailing spaces or extra blank lines

**Example (✅ Valid):**
```markdown
# Main Heading

## Sub-heading

- List item 1
- List item 2

**Bold text** and *italic text*

\`\`\`bash
# Code block
echo "hello"
\`\`\`
```

### 2. Heading Structure

**Rules:**
- Start with `# Title` (only one h1 per file)
- Use `##` for main sections
- Use `###` for subsections
- Don't skip heading levels (no jump from h2 to h4)
- Keep heading text concise (5-10 words max)

**Check:**
```markdown
# Main Title ← Only one h1
  (Content)
## Section 1
  (Content)
### Subsection
  (Content)
## Section 2
  (Content)
```

### 3. Link Validation

**Internal links:**
- Format: `[text](relative/path/to/file.md)` or `[text](../other-file.md)`
- Validate file exists at path
- Use relative paths (not absolute)
- No dead links

**External links:**
- Format: `[text](https://example.com)`
- Must start with `http://` or `https://`
- Optionally validate URLs respond with 200 OK

**Example (✅ Valid):**
```markdown
[See the guide](./getting-started.md)
[External resource](https://github.com/your-org/repo)
```

### 4. File Naming Conventions

**Standard pattern (customize for your repo):**
- Use kebab-case: `my-feature.md` (not `MyFeature.md` or `my_feature.md`)
- Lowercase: `how-to-guide.md` (not `How-To-Guide.md`)
- Descriptive names: `authentication-setup.md` (not `doc1.md`)
- Max 50 characters

**Example (✅ Valid names):**
```
getting-started.md
api-reference.md
troubleshooting-common-issues.md
release-notes-v2.5.md
```

### 5. Required Sections (by doc type)

**New Topic:**
- [ ] Title (h1)
- [ ] Overview/introduction
- [ ] Prerequisites (if applicable)
- [ ] Steps or sections
- [ ] Examples
- [ ] Troubleshooting

**Revision:**
- [ ] Only modified sections
- [ ] Updated dates/versions
- [ ] Links to related docs

**Release Notes:**
- [ ] Version number
- [ ] Date
- [ ] What's New
- [ ] Bug Fixes
- [ ] Breaking Changes (if any)
- [ ] Known Issues

**Troubleshooting:**
- [ ] Problem statement
- [ ] Cause/why it happens
- [ ] Solution/steps
- [ ] Prevention (if applicable)

### 6. Line Length & Formatting

**Standards:**
- Max 80 characters per line (for readability)
- One blank line between sections
- Two blank lines before new section
- No excessive blank lines (max 1 in sequence)

**Example (✅ Good):**
```markdown
# Section 1

Content here...

## Subsection

More content...
```

### 7. Code Blocks

**Format:**
```markdown
\`\`\`language
code here
\`\`\`
```

**Valid languages:**
- `bash`, `shell`, `sh`
- `python`, `js`, `typescript`, `java`, `go`, `ruby`
- `markdown`, `yaml`, `json`, `xml`, `html`, `css`
- `sql`, `graphql`
- `plaintext` (for generic code)

**Example (✅ Valid):**
\`\`\`bash
\`\`\`
\`\`\`python
def hello():
    print("world")
\`\`\`
\`\`\`

### 8. Consistency Checks

**Compare against existing docs:**
- ✅ Similar tone & voice
- ✅ Same terminology for same concepts
- ✅ Similar structure/organization
- ✅ Same code style conventions

**Questions to ask:**
- Does this match the style of similar docs?
- Are technical terms consistent?
- Is the tone appropriate for the audience?

## Validation Checklist

Use this before finalizing any document:

```
□ Markdown syntax valid (no errors)
□ Only one h1 heading
□ Heading hierarchy correct (no skipped levels)
□ All internal links valid (files exist)
□ All external links have correct format (http/https)
□ File name follows kebab-case convention
□ Line length < 80 characters
□ No excessive blank lines
□ Code blocks have language specified
□ Required sections present (based on type)
□ Tone matches existing docs
□ Technical terms consistent
□ No typos or grammar errors
```

## Agent Logic

```
FUNCTION validate_markdown(file_path):
  errors = []
  
  # Check syntax
  IF not valid_markdown_syntax(file_path):
    errors.append("Invalid Markdown syntax")
  
  # Check links
  FOR each link in file_path:
    IF link is internal:
      IF file doesn't exist:
        errors.append(f"Dead link: {link}")
    ELSE:
      IF link missing http/https:
        errors.append(f"Invalid URL format: {link}")
  
  # Check headings
  IF multiple h1 headings:
    errors.append("Multiple h1 headings found")
  IF heading_hierarchy_skipped():
    errors.append("Heading levels skipped")
  
  # Check naming
  IF not matches_kebab_case(filename):
    errors.append("Filename not kebab-case")
  
  # Check structure
  FOR section in required_sections:
    IF section missing:
      errors.append(f"Missing required section: {section}")
  
  IF errors is empty:
    RETURN "✅ Valid"
  ELSE:
    RETURN errors with line numbers
```

## Fixing Common Issues

### Issue: Dead Link
```markdown
❌ [See guide](non-existent-file.md)
✅ [See guide](./getting-started.md)
```

### Issue: Multiple h1 Headings
```markdown
❌ # Title
   Content...
   # Another Title

✅ # Title
   Content...
   ## Subsection
```

### Issue: Skipped Heading Levels
```markdown
❌ # Title
   ## Section
   #### Subsection  ← Skips h3!

✅ # Title
   ## Section
   ### Subsection
```

### Issue: Invalid Code Block
```markdown
❌ \`\`\`
code without language
\`\`\`

✅ \`\`\`bash
code with language
\`\`\`
```

## Customization

1. **Add your validation rules** - Edit the checklist above
2. **Adjust line length** - Change 80 char limit if needed
3. **Define required sections** - Add/remove based on your doc types
4. **Add custom conventions** - Company-specific standards

## Related Skills

- **github-aware-writing** - Ensure content is written correctly before validation
- **determine-content-scope** - Know doc type to validate against correct checklist
