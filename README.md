---

# Setting Up Biome, Husky, and Pre-Commit for GitHub

This guide will walk you through setting up **Biome** for code linting and formatting, **Husky** for Git hooks, and a pre-commit hook to ensure code consistency before pushing changes to GitHub.

---

## Prerequisites

Before starting, make sure you have:

- **Node.js** (preferably the latest LTS version)
- **npm** (comes with Node.js)

---

## 1. Install Biome

Biome handles linting and formatting. To install it, run:

```bash
npm install --save-dev @biomejs/biome
```

---

## 2. Configure Biome

Create a `biome.json` file in the root directory to configure Biome’s linting, formatting, and import organization settings.

Here’s an example `biome.json` configuration file:

```json
{
  "$schema": "https://biomejs.dev/schemas/1.6.4/schema.json",
  "vcs": {
    "clientKind": "git",
    "enabled": true,
    "useIgnoreFile": true,
    "defaultBranch": "main"
  },
  "files": {
    "ignore": ["src/generated/*.ts"]
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true,
      "complexity": {
        "noForEach": "off"
      },
      "suspicious": {
        "noCommentText": "warn",
        "noExplicitAny": "warn",
        "noShadowRestrictedNames": "warn"
      }
    }
  },
  "formatter": {
    "enabled": true,
    "formatWithErrors": false,
    "indentStyle": "space",
    "indentWidth": 2,
    "lineEnding": "lf",
    "lineWidth": 80,
    "attributePosition": "auto"
  },
  "organizeImports": {
    "enabled": true
  },
  "javascript": {
    "formatter": {
      "jsxQuoteStyle": "double",
      "quoteProperties": "asNeeded",
      "trailingCommas": "all",
      "semicolons": "asNeeded",
      "arrowParentheses": "always",
      "bracketSpacing": true,
      "bracketSameLine": false,
      "quoteStyle": "single",
      "attributePosition": "auto"
    }
  },
  "json": {
    "formatter": {
      "indentWidth": 2
    }
  }
}
```

---

## 3. Add Biome Commands to `package.json`

Add the following scripts in your `package.json` to allow easy access to Biome and Husky commands:

```json
"scripts": {
  "format": "npx @biomejs/biome format --write ./",
  "prepare": "husky install",
  "pre-commit": "npm run prepare && npm run format && git add ."
}
```

### Script Explanation:

- **format**: Formats all files using Biome.
- **prepare**: Initializes Husky and sets up Git hooks.
- **pre-commit**: Formats code, adds formatted files to the commit, and prepares for the commit.

---

## 4. Install Husky

Husky is used to manage Git hooks.

1. Install Husky:

   ```bash
   npm install --save-dev husky
   ```

2. Set up Husky to create the `.husky` directory:

   ```bash
   npx husky init
   ```

---

## 5. Add a Pre-Commit Hook with Husky

Create a `pre-commit` hook in Husky that will format code with Biome before committing:

```bash
npx husky add .husky/pre-commit "npm run pre-commit"
```

This will create a file at `.husky/pre-commit` with the following contents:

```bash
npm run pre-commit
```

With this setup, every time you attempt to commit, the `pre-commit` script will format your code and add any changes to the commit.

---

## 6. Verify the Setup

1. **Make a test commit** to see if the pre-commit hook works as expected.
2. Any staged changes will be automatically formatted before they are committed.

---

## Usage

- **Manually format files** with:

  ```bash
  npm run format
  ```

- **Verify pre-commit**: Stage a file, make changes, and try committing. Your code will be automatically formatted before the commit is completed.

---

This setup will enforce code standards with every commit, ensuring consistency in code style and formatting across your project.

---
