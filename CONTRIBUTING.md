# Contributing to dev-resources

Thank you for considering contributing! This repo is meant to be a clean, high-quality reference for developers. Please read the guidelines below before submitting.

---

## How to Contribute

### 1. Fork the Repository
```bash
git clone https://github.com/Ranahmad1/dev-resources.git
cd dev-resources
```

### 2. Create a Branch
```bash
git checkout -b add/typescript-cheatsheet
# or
git checkout -b fix/css-grid-example
```

### 3. Make Your Changes
- Add content to existing files, or create a new `.md` file in `cheatsheets/`
- Add new templates to `templates/` as `.html` files
- Keep formatting consistent with existing files

### 4. Commit & Push
```bash
git add .
git commit -m "add: TypeScript cheatsheet"
git push origin add/typescript-cheatsheet
```

### 5. Open a Pull Request
- Go to the repo on GitHub
- Click **"Compare & pull request"**
- Fill in a clear title and description

---

## Contribution Guidelines

### ✅ Good Contributions
- New cheatsheet for a language/tool not already covered
- Fixing wrong code examples
- Adding missing useful snippets
- Improving explanations
- New HTML/CSS templates (must be clean, responsive, no dependencies)

### ❌ Please Avoid
- Adding bloated or opinionated frameworks
- Copy-pasting content from other sources without attribution
- Low quality or duplicate content
- Broken code examples

---

## File Naming Conventions

```
cheatsheets/typescript.md       ✅
cheatsheets/TypeScript.MD       ❌
templates/dashboard-ui.html     ✅
templates/my cool template.html ❌
```

---

## Commit Message Format

```
add:    new cheatsheet or template
fix:    correct wrong example or typo
update: improve existing content
remove: delete outdated content
```

---

## Questions?

Open an issue or reach out via [GitHub](https://github.com/Ranahmad1).
