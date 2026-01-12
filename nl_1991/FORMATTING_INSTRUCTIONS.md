# Newsletter Formatting Instructions

This document contains strategies and patterns for fixing up MCCC newsletter markdown files converted from PDFs.

## Overview of Common Issues

Newsletter files converted from PDF typically have:
- Tables rendered as scattered text with excessive line breaks
- Lists that should be bulleted appearing as plain text
- Inconsistent formatting across different monthly files
- Officer/contact information spread across multiple lines

## Target Sections to Format

### 1. Board of Directors
**Search Pattern:** `"Board of Directors"`

**Target Format:** 3-column markdown table
```markdown
| Name | Position | Phone Number |
|------|----------|-------------|
| John Doe | President | 817-555-1234 |
```

**Original Pattern (typical):**
```
Name

Position

Phone Number

Name2

Position2

Phone Number2
```

**Strategy:**
- Use `grep_search` to find all instances
- Read file to see exact formatting and line numbers
- Use `multi_replace_string_in_file` to batch process multiple files
- Include 3-5 lines of context before/after for unique matching

### 2. Chapter Officers
**Search Pattern:** `"Chapter Officers"`

**Target Format:** Same as Board of Directors - 3-column table
```markdown
| Name | Position | Phone Number |
|------|----------|-------------|
```

**Note:** Sometimes combined with "President" or similar titles in the same block

### 3. Calendar of Events
**Search Pattern:** `"Calendar of Events"` or `"Calendar Of Events"`

**Target Format:** 3-column markdown table
```markdown
| Date | Event | Location |
|------|-------|----------|
| January 11 | MCCC Board Meeting | Location details |
```

**Original Pattern:**
```
Date

Event name

Location

Date2

Event name2

Location2
```

### 4. Normal Meeting Schedule
**Search Pattern:** `"Normal Meeting Schedule"`

**Target Format:** Bulleted list with bold days
```markdown
- **1st Saturday** - MCCC Board of Directors
- **2nd Tuesday** - Amiga By-The-Loop Chapter
- **3rd Wednesday** - Amiga North Dallas Chapter
```

**Original Pattern:**
```
1st Saturday

MCCC Board of Directors

2nd Tuesday

Amiga By-The-Loop Chapter
```

**Important Notes:**
- Some files may already be formatted (skip these)
- Some files may have empty sections (heading exists but no content)
- Check actual file content if batch replacements fail

### 5. SIGS (Special Interest Groups)
**Search Pattern:** `"SIGS"` or `"## SIGS"`

**Target Format:** 3-column table
```markdown
| Name | Contact | Phone Number |
|------|---------|--------------|
| Programmer SIG | Kevin Wambganz | 214-414-9460 |
| VidSIG | Jack DeTore | 817-275-3762 |
```

**Original Pattern (varies widely):**
```
Programmer SIG Kevin Wambganz

214-414-9460

VidSIG

Jack DeTore

817-275-3762
```

or

```
Programmer SIG Kevin Wambganz 214-414-9460

VidSIG Jack DeTore 817-275-3762
```

### 6. Election Results
**Search Pattern:** `election|elected|officer.*result|voting.*result` (regex)

**Target Format:** 2-column table
```markdown
| Position | Name |
|----------|------|
| President | John Doe |
| Vice President | Jane Smith |
| Treasurer | Bob Johnson |
```

**Original Patterns (highly variable):**
```
President

John Doe

Vice President

Jane Smith
```

or

```
President - John Doe
Vice-President - Jane Smith
Treasurer - Bob Johnson
```

or

```
President John Doe Vice-President Jane Smith
```

**Strategy:** Use regex search first, then read each match to determine exact format

## Workflow Steps

### Phase 1: Discovery
1. Use `grep_search` with appropriate pattern to find all instances across files
2. Note which files contain the target sections and approximate line numbers
3. Use `read_file` to examine 1-2 examples to understand exact formatting

### Phase 2: Batch Processing
1. Group files with similar formatting patterns
2. Use `multi_replace_string_in_file` to process multiple files at once
3. Include enough context (3-5 lines before/after) to make oldString unique

### Phase 3: Handle Failures
1. If batch replacements fail, use `read_file` to check actual content
2. Common reasons for failure:
   - File already formatted from previous pass
   - Section is empty (heading exists but no content)
   - Text varies slightly from expected pattern
3. Fix individually with `replace_string_in_file` using exact text

### Phase 4: Verification
1. Spot check a few files to ensure formatting is correct
2. Look for any files that might have been missed
3. Check for edge cases (empty sections, already formatted, etc.)

## Search Strategies

### Finding Sections
```
grep_search with isRegexp=false for exact text:
- "Board of Directors"
- "Chapter Officers"  
- "Calendar of Events"
- "Normal Meeting Schedule"
- "SIGS"
```

### Finding Election Results
```
grep_search with isRegexp=true:
- "election|elected|officer.*result|voting.*result"
```

### Checking Specific Files
```
Use includePattern parameter:
- includePattern: "1991_*.md"
- includePattern: "1991_02.md" (for single file)
```

## Common Pitfalls

1. **Not enough context in oldString**: Replacements fail if the text isn't unique
   - Solution: Include 3-5 lines before and after target text

2. **Variations between files**: Same section formatted differently in different months
   - Solution: Read files individually, process in smaller batches with similar patterns

3. **Already formatted sections**: Previous work may have already fixed some files
   - Solution: When replacement fails, read the file to check current state

4. **Empty sections**: Heading exists but no content between it and next section
   - Solution: Skip these files - nothing to format

5. **Special characters**: HTML entities like `&amp;` in names or text
   - Solution: Copy exact text including entities from read_file output

## File Naming Patterns

Newsletters typically follow: `YEAR_MONTH.md`
- Example: `1991_01.md` through `1991_12.md`
- Artifacts in directories: `1991_01_artifacts/`

## Multi-Replace Example

```json
{
  "explanation": "Format Board of Directors tables for files 01-04",
  "replacements": [
    {
      "explanation": "Format Board of Directors in 1991_01.md",
      "filePath": "/full/path/to/1991_01.md",
      "oldString": "## MCCC Board of Directors\n\nName1\n\nPosition1\n\nPhone1\n\n...(with context)",
      "newString": "## MCCC Board of Directors\n\n| Name | Position | Phone Number |\n|------|----------|-------------|\n| Name1 | Position1 | Phone1 |"
    },
    ...
  ]
}
```

## Summary Checklist

For each newsletter directory:
- [ ] Fix Board of Directors tables (3-column)
- [ ] Fix Chapter Officers tables (3-column)  
- [ ] Fix Calendar of Events (3-column table with Date/Event/Location)
- [ ] Fix Normal Meeting Schedule (bulleted list with bold dates)
- [ ] Fix SIGS sections (3-column table)
- [ ] Search for and fix Election Results (2-column table)
- [ ] Verify no sections were missed
- [ ] Check for edge cases (empty, already formatted)

## Time-Saving Tips

1. Always start with grep_search to get full picture
2. Use multi_replace when files have consistent patterns (batch by 4-5 files)
3. Read file content when replacements fail rather than guessing
4. Process in phases: all Board sections, then all Officers, then all Calendars, etc.
5. Keep track of which files succeed/fail to avoid duplicate work

## Expected Results

After formatting:
- All tables render properly in markdown viewers
- Lists are properly bulleted with consistent formatting
- Information is easy to scan and read
- Structure is consistent across all files in the directory
