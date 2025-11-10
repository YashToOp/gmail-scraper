# Contributing to Gmail Scraper Pro

Thank you for your interest in contributing! This project is primarily educational, demonstrating web automation and bot detection bypass techniques.

## 🎯 Project Goals

- Educational demonstration of Puppeteer automation
- Showcase anti-detection techniques (stealth plugin)
- Clean, well-documented codebase
- Real-world full-stack architecture patterns

## 🤝 How to Contribute

### 1. Fork & Clone

```bash
# Fork on GitHub first, then:
git clone https://github.com/YOUR_USERNAME/gmail-scraper-pro.git
cd gmail-scraper-pro
```

### 2. Create a Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/issue-description
```

**Branch naming conventions:**
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation updates
- `refactor/` - Code improvements
- `test/` - Test additions

### 3. Set Up Development Environment

```bash
# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

### 4. Make Your Changes

- Write clean, readable code
- Follow existing code style
- Add comments for complex logic
- Update documentation if needed

### 5. Test Your Changes

```bash
# Test backend
cd backend
npm start

# Test frontend (separate terminal)
cd frontend
npm run dev

# Manual testing
# Try scraping 10-50 emails to verify functionality
```

### 6. Commit & Push

```bash
git add .
git commit -m "feat: add amazing feature"
# or
git commit -m "fix: resolve pagination issue"

git push origin feature/your-feature-name
```

**Commit message format:**
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation only
- `style:` - Formatting, missing semicolons, etc.
- `refactor:` - Code change that neither fixes nor adds feature
- `test:` - Adding tests
- `chore:` - Updating build tasks, configs, etc.

### 7. Open Pull Request

1. Go to your fork on GitHub
2. Click "Pull Request"
3. Select base: `main` ← compare: `your-branch`
4. Fill out the PR template
5. Wait for review

## 📋 Pull Request Guidelines

### PR Checklist
- [ ] Code follows project style
- [ ] No console.log() left in production code (use proper logging)
- [ ] Comments added for complex logic
- [ ] Documentation updated (if needed)
- [ ] Tested manually with 10-50 emails
- [ ] No breaking changes (or clearly documented)
- [ ] No sensitive data committed (.env, credentials, etc.)

### PR Description Should Include
- **What**: Brief description of changes
- **Why**: Reason for the change
- **How**: Technical approach (if complex)
- **Testing**: How you tested it
- **Screenshots**: If UI changes

Example:
```markdown
## What
Added retry mechanism for pagination clicks

## Why
Sometimes the "Older" button doesn't respond on first click

## How
- Implemented 3-tier click strategy (marked element → aria-label → XPath)
- Added scroll-into-view before clicking
- Random delays between retries

## Testing
Tested with 201 emails (6 pages), all pagination successful
```

## 🐛 Reporting Bugs

### Before Reporting
1. Search existing issues
2. Test with latest `main` branch
3. Verify it's not a configuration issue

### Bug Report Template

**Title**: Short, descriptive title

**Description**:
- What you expected
- What actually happened
- Steps to reproduce

**Environment**:
- OS: (Windows 10, macOS 14, Ubuntu 22.04)
- Node version: `node --version`
- npm version: `npm --version`
- Browser: (Chrome 120, Edge, etc.)

**Logs**:
```
Paste relevant error logs here
```

**Screenshots**:
Attach if applicable

## 💡 Feature Requests

We welcome ideas! Please include:
- **Use case**: What problem does it solve?
- **Proposed solution**: How would it work?
- **Alternatives**: Other approaches you considered
- **Impact**: Who benefits from this?

## 🏗️ Areas Needing Help

### High Priority
- [ ] Better Gmail selector robustness (UI changes)
- [ ] Unit tests for critical functions
- [ ] Performance optimizations (parallel processing)
- [ ] Better error messages

### Medium Priority
- [ ] MongoDB integration for history
- [ ] Queue system (Bull + Redis)
- [ ] Scheduled scraping (cron jobs)
- [ ] Export to JSON/Excel formats

### Low Priority
- [ ] Dark mode for frontend
- [ ] Dashboard with analytics
- [ ] Multi-language support
- [ ] Docker containerization

## 📝 Code Style

### JavaScript/JSX
- Use ES6+ features (const, arrow functions, async/await)
- 2 spaces for indentation
- Semicolons required
- Single quotes for strings
- Descriptive variable names

**Good:**
```javascript
const scrapedEmails = await extractEmailsFromPage(page, currentPage);
const filteredResults = filterEmails(scrapedEmails, requestedCount);
```

**Avoid:**
```javascript
var e = await f(p, cp);
let r = g(e, rc);
```

### Comments
- Explain **why**, not **what**
- Use JSDoc for functions

```javascript
/**
 * Wait for email list to change after pagination click
 * Uses page.waitForFunction for efficient in-browser polling
 * 
 * @param {Page} page - Puppeteer page instance
 * @param {string} prevThreadId - Last visible thread ID before click
 * @param {number} timeout - Max wait time in milliseconds
 * @returns {Promise<{changed: boolean, newThreadId: string}>}
 */
async function waitForListChange(page, prevThreadId, timeout) {
  // Implementation...
}
```

### File Organization
- One component/function per file
- Group related utilities in `/utils`
- Keep routes separate in `/routes`
- Puppeteer logic in `/puppeteer`

## 🧪 Testing Guidelines

### Manual Testing Checklist
- [ ] Test with 10 emails (quick smoke test)
- [ ] Test with 50 emails (1 page)
- [ ] Test with 100-200 emails (pagination)
- [ ] Test headless mode
- [ ] Test with invalid credentials (error handling)
- [ ] Check CSV output format
- [ ] Verify no console errors in browser

### Future: Automated Tests
We plan to add:
- Jest for unit tests
- Playwright for E2E tests
- CSV validation tests

## 🚫 What We Don't Accept

- ❌ Malicious features (keyloggers, data theft, etc.)
- ❌ Credential hardcoding
- ❌ Removing security warnings/disclaimers
- ❌ Features that violate laws (GDPR, CFAA, etc.)
- ❌ Unethical use cases (spam, harassment)
- ❌ Large binary files (screenshots, videos)
- ❌ Breaking changes without discussion

## 📜 Code of Conduct

### Our Standards
- ✅ Be respectful and inclusive
- ✅ Welcome newcomers
- ✅ Accept constructive criticism
- ✅ Focus on what's best for the project
- ❌ No harassment, trolling, or personal attacks
- ❌ No spam or self-promotion

### Enforcement
Violations may result in:
1. Warning
2. Temporary ban
3. Permanent ban

Report issues to project maintainers.

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

## 🙏 Recognition

Contributors will be:
- Added to CONTRIBUTORS.md
- Mentioned in release notes
- Thanked in README.md

## 💬 Questions?

- Open a GitHub Discussion
- Comment on relevant issues
- Check existing documentation

---

**Thank you for making this project better!** 🎉

Every contribution, no matter how small, is appreciated. Whether it's fixing a typo, improving documentation, or adding a major feature - we value your effort!
