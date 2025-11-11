# CI/CD Documentation Quality Checks

This repository uses GitHub Actions to automatically validate documentation quality on every push and pull request.

## What Gets Checked

### 1. **Markdown Linting** (`markdownlint`)
- Ensures consistent markdown formatting
- Checks heading styles, list indentation, line length
- Config: `.markdownlint.json`

### 2. **Link Checking**
- Validates all internal and external links
- Catches broken links, 404s, and typos
- Config: `.github/workflows/mlc_config.json`

### 3. **Spell Checking**
- Checks spelling across all markdown files
- Uses custom wordlist for technical terms
- Config: `.github/workflows/spellcheck-config.yml`
- Wordlist: `.github/workflows/wordlist.txt`

### 4. **Documentation Structure**
- Verifies all required files exist
- Checks for common broken link patterns
- Validates directory structure

### 5. **JSON Validation**
- Extracts JSON code blocks from markdown
- Validates JSON syntax
- Ensures configuration examples are correct

### 6. **Social Links Verification**
- Checks GitHub repository links
- Ensures links point to correct repos
- Validates community repo references

## Running Checks Locally

### Markdown Linting
```bash
# Install markdownlint-cli2
npm install -g markdownlint-cli2

# Run checks
markdownlint-cli2 "**/*.md"
```

### Link Checking
```bash
# Install markdown-link-check
npm install -g markdown-link-check

# Check a specific file
markdown-link-check README.md

# Check all files
find . -name "*.md" -not -path "./node_modules/*" -exec markdown-link-check {} \;
```

### Spell Checking
```bash
# Install pyspelling
pip install pyspelling

# Run spell check
pyspelling -c .github/workflows/spellcheck-config.yml
```

## Adding Words to Dictionary

If the spell checker flags a valid technical term or proper noun:

1. Edit `.github/workflows/wordlist.txt`
2. Add the word (one per line)
3. Commit and push

## Workflow Status

The workflow runs on:
- ✅ Push to `main` branch (if docs changed)
- ✅ Pull requests to `main` (if docs changed)
- ✅ Manual trigger via GitHub Actions UI

## Viewing Results

1. Go to **Actions** tab in GitHub
2. Click on **Documentation Quality** workflow
3. View individual check results
4. See summary report at bottom

## Fixing Issues

### Markdown Linting Errors
Common fixes:
- Use consistent heading styles (`# Heading`)
- Fix list indentation (2 spaces)
- Add blank lines around code blocks
- Remove trailing spaces

### Broken Links
- Check for typos in link paths
- Verify file names and locations
- Use relative paths correctly (`./file.md`, not `file.md`)
- Avoid duplicate paths (`setup/setup/` → `setup/`)

### Spelling Errors
- Fix actual misspellings
- Add technical terms to wordlist
- Check for capitalization issues

### JSON Errors
- Validate JSON in code blocks
- Check for missing commas
- Ensure proper bracket matching
- Use [jsonlint.com](https://jsonlint.com/) to test

## Configuration Files

| File | Purpose |
|------|---------|
| `.markdownlint.json` | Markdown linting rules |
| `.github/workflows/docs-quality.yml` | Main CI/CD workflow |
| `.github/workflows/mlc_config.json` | Link checker config |
| `.github/workflows/spellcheck-config.yml` | Spell checker config |
| `.github/workflows/wordlist.txt` | Custom dictionary |

## Best Practices

### Writing Documentation
- ✅ Use relative links for internal docs
- ✅ Test links before committing
- ✅ Add alt text to images
- ✅ Keep JSON examples valid
- ✅ Use consistent formatting

### Adding New Files
- Update required files list in workflow if needed
- Follow existing naming conventions
- Link new docs from main README
- Add to navigation where appropriate

### Pull Requests
- CI checks run automatically
- Fix any failing checks before merging
- Review summary report for issues
- Ask for help if stuck on a check

## Troubleshooting

### "Markdown Linting Failed"
```bash
# Run locally to see specific errors
markdownlint-cli2 "**/*.md"

# Auto-fix some issues
markdownlint-cli2-fix "**/*.md"
```

### "Link Check Failed"
```bash
# Check specific file
markdown-link-check docs/setup/CLAUDE-DESKTOP.md

# Common issues:
# - Relative path wrong: ./file.md (same dir) vs ../file.md (parent dir)
# - File doesn't exist: Check spelling and location
# - External link down: Wait and retry, or update link
```

### "Spell Check Failed"
```bash
# Run locally
pyspelling -c .github/workflows/spellcheck-config.yml

# Add word to dictionary if it's valid
echo "wordname" >> .github/workflows/wordlist.txt
```

## Disabling Checks

To skip CI checks for a commit (use sparingly):
```bash
git commit -m "docs: update README [skip ci]"
```

## Questions?

Open an issue or check the [GitHub Actions docs](https://docs.github.com/en/actions).
