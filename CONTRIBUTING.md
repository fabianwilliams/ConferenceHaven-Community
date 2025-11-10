# Contributing to ConferenceHaven

Thank you for your interest in contributing to ConferenceHaven! 🎉

This is the **Community Repository** for feedback, documentation, and discussions. The core codebase is in a private repository, but we welcome many types of contributions here.

---

## 🤝 How You Can Contribute

### 1. Report Bugs
Found a bug? Please help us fix it!

- **Use the bug report template**: [Report a bug](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=bug_report.md)
- Include as much detail as possible:
  - What you were trying to do
  - What actually happened
  - Steps to reproduce
  - Your setup (AI client, OS, etc.)
  - Screenshots if helpful

### 2. Request Features
Have an idea for a new feature?

- **Use the feature request template**: [Request a feature](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=feature_request.md)
- Describe:
  - The problem you're trying to solve
  - Your proposed solution
  - Alternative approaches you've considered
  - How it benefits other users

### 3. Request a Conference
Want your conference added to ConferenceHaven?

- **Use the conference request template**: [Request a conference](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=conference_request.md)
- Include:
  - Conference name, dates, location
  - Website URL
  - Session data source (Sched, Sessionize, CSV, etc.)
  - Contact email for coordination

### 4. Improve Documentation
Help make our docs better!

**What you can do**:
- Fix typos or unclear instructions
- Add examples or clarifications
- Update outdated information
- Add new troubleshooting tips
- Improve setup guides

**How to contribute docs**:
1. Fork this repository
2. Make your changes
3. Submit a pull request
4. Explain what you improved and why

**Documentation files**:
- `README.md` - Main overview
- `docs/SETUP-GUIDES.md` - Client setup instructions
- `docs/FAQ.md` - Common questions
- `docs/TROUBLESHOOTING.md` - Problem solving
- `docs/ARCHITECTURE.md` - Technical overview

### 5. Share Your Experience
Help others learn from your experience!

- **Start a discussion**: [Discussions](https://github.com/fabianwilliams/ConferenceHaven-Community/discussions)
- Share:
  - Use cases and workflows
  - Integration examples
  - Tips and tricks
  - Conference experiences
  - AI assistant comparisons

### 6. Help Others
Answer questions and provide support!

- Monitor [Issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- Participate in [Discussions](https://github.com/fabianwilliams/ConferenceHaven-Community/discussions)
- Share troubleshooting tips
- Help new users get started

---

## 📝 Contribution Guidelines

### Issue Etiquette

**Before opening an issue**:
- Search existing issues to avoid duplicates
- Check if it's already documented in FAQ or Troubleshooting
- Try the latest version (in case it's already fixed)

**When writing issues**:
- Use the appropriate template
- Be clear and specific
- Include reproduction steps
- Add screenshots/logs when helpful
- Be respectful and constructive

**After opening an issue**:
- Respond to follow-up questions
- Update if you find new information
- Close if you solve it yourself (and share the solution!)

### Pull Request Process

**For documentation changes**:

1. **Fork the repository**:
   ```bash
   git clone https://github.com/YOUR_USERNAME/ConferenceHaven-Community.git
   cd ConferenceHaven-Community
   ```

2. **Create a branch**:
   ```bash
   git checkout -b docs/improve-setup-guide
   ```

3. **Make your changes**:
   - Edit the relevant files
   - Preview Markdown locally
   - Check for typos and formatting

4. **Commit with a clear message**:
   ```bash
   git add docs/SETUP-GUIDES.md
   git commit -m "docs: clarify Claude Desktop setup steps for Windows"
   ```

5. **Push and create PR**:
   ```bash
   git push origin docs/improve-setup-guide
   ```
   Then open a pull request on GitHub.

**PR Guidelines**:
- Keep changes focused (one topic per PR)
- Write a clear description of what changed and why
- Reference related issues if applicable
- Respond to review feedback
- Be patient - reviews may take a few days

### Commit Message Format

Use conventional commits format:

```
<type>: <subject>

<body (optional)>
```

**Types**:
- `docs:` - Documentation changes
- `fix:` - Bug fix in documentation
- `feat:` - New documentation section
- `chore:` - Maintenance (updating links, etc.)

**Examples**:
```
docs: add LM Studio troubleshooting steps

Added section covering common issues with LM Studio MCP setup,
including model compatibility and connection debugging.

Closes #42
```

```
fix: correct Azure SQL connection string in architecture

The example connection string had the wrong format.
Updated to match current Azure SQL format.
```

### Markdown Style Guide

**For consistency across documentation**:

1. **Headings**:
   - Use `#` for title (one per doc)
   - Use `##` for main sections
   - Use `###` for subsections
   - Use `####` sparingly

2. **Code blocks**:
   ```bash
   # Always specify language
   # Include comments for context
   ```

3. **Links**:
   - Use relative links for internal docs: `[Setup](SETUP-GUIDES.md)`
   - Use full URLs for external links: `[Node.js](https://nodejs.org)`

4. **Lists**:
   - Use `-` for unordered lists
   - Use `1.` for ordered lists (Markdown auto-numbers)

5. **Emphasis**:
   - Use `**bold**` for important terms
   - Use `*italic*` for emphasis
   - Use `code` for commands, filenames, code terms

6. **Examples**:
   - Show both ❌ wrong and ✅ correct examples when helpful
   - Include real-world examples, not just placeholders

---

## 🔒 What's Not in This Repo

The **core codebase** is in a private repository for:
- MCP server implementation
- Backend agent code
- Database migrations
- Infrastructure as Code (Bicep/Terraform)
- CI/CD pipelines
- Security-sensitive configurations

**Why private?**
- Contains proprietary algorithms and optimizations
- Includes production secrets and configurations
- Requires Azure resources and API keys
- Organizer-specific features and analytics

**Want access to the core codebase?**
- For conference organizers: Email conferencehaven@adotob.com
- For contributors: Demonstrate significant community contributions first
- For security researchers: Reach out about responsible disclosure

---

## 🏆 Recognition

Contributors to this Community repository will be:
- Listed in the Contributors section (coming soon)
- Acknowledged in release notes for significant contributions
- Potentially invited to early access programs
- Considered for core codebase contributor access

---

## 📞 Questions?

- **General questions**: [Open a discussion](https://github.com/fabianwilliams/ConferenceHaven-Community/discussions)
- **Bug reports**: [Open an issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=bug_report.md)
- **Private inquiries**: conferencehaven@adotob.com

---

## 📜 Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for everyone, regardless of:
- Background or identity
- Experience level
- Nationality or location
- Education
- Socioeconomic status
- Gender identity or expression
- Sexual orientation
- Age
- Disability
- Religion or beliefs
- Any other characteristic

### Our Standards

**Positive behavior**:
- Being respectful and professional
- Welcoming newcomers and helping them
- Accepting constructive criticism gracefully
- Focusing on what's best for the community
- Showing empathy towards others

**Unacceptable behavior**:
- Harassment, discrimination, or hate speech
- Trolling, insulting, or derogatory comments
- Personal or political attacks
- Public or private harassment
- Publishing others' private information
- Spam or off-topic comments
- Any conduct inappropriate in a professional setting

### Enforcement

Violations can be reported to conferencehaven@adotob.com. All reports will be reviewed and investigated.

Consequences may include:
- Warning
- Temporary ban
- Permanent ban

We reserve the right to remove, edit, or reject contributions that violate this Code of Conduct.

### Attribution

This Code of Conduct is adapted from the [Contributor Covenant](https://www.contributor-covenant.org/), version 2.1.

---

## 🙏 Thank You!

Every contribution, no matter how small, makes ConferenceHaven better for everyone. Whether you're reporting a bug, improving docs, or helping other users, we appreciate you! 💙

---

**Last updated**: November 9, 2025
