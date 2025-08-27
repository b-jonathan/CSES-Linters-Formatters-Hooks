# JavaScript/TypeScript

## Table of Contents
- [JavaScript/TypeScript](#javascripttypescript)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Initial Setup](#initial-setup)
    - [ESLint (Flat Config)](#eslint-flat-config)
    - [Prettier](#prettier)
    - [Ignore Files](#ignore-files)
      - [`.eslintignore`](#eslintignore)
      - [`.prettierignore`](#prettierignore)
    - [Husky Git Hooks](#husky-git-hooks)
  - [Scripts](#scripts)
  - [Usage](#usage)
  - [Example Project Structure](#example-project-structure)
    - [1. Pure Monorepo (single app)](#1-pure-monorepo-single-app)
    - [2. Split Frontend/Backend](#2-split-frontendbackend)
  - [Example Husky Pre-commit Hook (Interactive Lint-Fix)](#example-husky-pre-commit-hook-interactive-lint-fix)
    - [Adapting for JavaScript vs TypeScript](#adapting-for-javascript-vs-typescript)
  - [Resources](#resources)

---

## Introduction

Maintaining code quality is crucial for any project. This guide covers:
- **ESLint**: Finds bugs, enforces best practices, and keeps code maintainable.
- **Prettier**: Ensures consistent code style, reducing friction in code reviews and collaboration.
- **Husky**: Automates checks before commits, preventing bad code from entering your repository.

These tools help teams focus on building features, not debating style or fixing preventable bugs late in the process.

---

## Initial Setup

### ESLint (Flat Config)

**Why ESLint?**
ESLint analyzes your code for errors, anti-patterns, and style issues. It helps catch bugs early and enforces standards, making your codebase more reliable and easier to maintain.

1. Install ESLint:
   ```bash
   npm install --save-dev eslint
   ```
2. Create `eslint.config.js` in your project root:
   ```js
   // eslint.config.js
   import js from '@eslint/js';

   export default [
     js.configs.recommended,
     {
       rules: {
         'semi': ['error', 'always'],
         'quotes': ['error', 'single'],
       },
     },
   ];
   ```
   > If using ES modules, add `"type": "module"` to your `package.json`.

3. Run ESLint:
   ```bash
   npx eslint . --config eslint.config.js
   ```

### Prettier

**Why Prettier?**
Prettier automatically formats your code, ensuring a consistent style across your team. This reduces time spent on formatting debates and makes code reviews more focused on logic.

1. Install Prettier:
   ```bash
   npm install --save-dev prettier
   ```
2. Create `.prettierrc`:
   ```json
    {
    "tabWidth": 2,
    "semi": true,
    "singleQuote": false,
    "trailingComma": "all",
    }
   ```

### Ignore Files

**Why Ignore Files?**
Ignore files prevent unnecessary or generated files (like dependencies and build artifacts) from being linted or formatted, speeding up checks and avoiding false positives.

#### `.eslintignore`
```
node_modules/
dist/
build/
coverage/
*.log
*.tmp
```

#### `.prettierignore`
```
node_modules/
dist/
build/
coverage/
*.log
*.tmp
```

### Husky Git Hooks

**Why Husky?**
Husky automates pre-commit and pre-push checks, ensuring that only code passing lint and format checks makes it into your repository. This protects your main branch from preventable errors and enforces team standards automatically.

1. Install Husky:
   ```bash
   npm install --save-dev husky
   npx husky init
   ```
2. Add to `package.json` scripts:
   ```json
   "scripts": {
     "prepare": "husky init"
   }
   ```
3. Add a pre-commit hook:
   ```bash
   npx husky add .husky/pre-commit "npm run lint-check"
   ```

---

## Scripts

Add these scripts to your `package.json` for easy linting and formatting:

```json
"scripts": {
  "format": "prettier --write .",
  "lint-fix": "eslint --fix . && prettier --write .",
  "lint-check": "eslint . && prettier --check ."
}
```

---

## Usage

1. Write code as usual.
2. Run `npm run format` to format code.
3. Run `npm run lint-fix` to auto-fix lint and format issues.
4. Run `npm run lint-check` to check for lint and format errors (used by Husky pre-commit).
5. Commit your changes—Husky will run the pre-commit hook.

---


---


## Example Project Structure

After following this guide, your project might look like one of the following:

### 1. Pure Monorepo (single app)
```
my-app/
├── node_modules/
├── src/
│   └── ...
├── .eslintignore
├── .prettierignore
├── .prettierrc
├── eslint.config.js
├── package.json
├── .husky/
│   └── pre-commit
└── ...
```

### 2. Split Frontend/Backend
```
my-app/
├── node_modules/
├── frontend/
│   ├── src/
│   │   └── ...
│   ├── .eslintignore
│   ├── .prettierignore
│   ├── .prettierrc
│   ├── eslint.config.js
│   ├── package.json
│   └── tsconfig.json (if using TypeScript)
├── backend/
│   ├── src/
│   │   └── ...
│   ├── .eslintignore
│   ├── .prettierignore
│   ├── .prettierrc
│   ├── eslint.config.js
│   ├── package.json
│   └── tsconfig.json (if using TypeScript)
├── .husky/
│   └── pre-commit
├── package.json (optionally, for root scripts or dependencies)
└── ...
```

---

## Example Husky Pre-commit Hook (Interactive Lint-Fix)

You can make your `.husky/pre-commit` script interactive, so if lint errors are found, it offers to run `lint-fix` automatically and asks you to recommit once fixed. Example (bash):

```bash
npm run lint-check
if [ $? -ne 0 ]; then
   echo "Lint errors detected. Would you like to run lint-fix automatically? (y/n)"
   read answer
   if [ "$answer" = "y" ]; then
      npm run lint-fix
      echo "If all errors are fixed, please recommit your changes."
      exit 1
   else
      echo "Commit aborted due to lint errors."
      exit 1
   fi
fi
```

This ensures you never commit code with lint errors, and gives you a quick way to fix and recommit.

### Adapting for JavaScript vs TypeScript

- **JavaScript:** Use the setup above as-is.
- **TypeScript:**
   - Install additional packages:
      ```bash
      npm install --save-dev typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
      ```
   - Update your `eslint.config.js` to include TypeScript support. Example:
      ```js
      import js from '@eslint/js';
      import tseslint from 'typescript-eslint';

      export default [
         js.configs.recommended,
         tseslint.configs.recommended,
         {
            rules: {
               'semi': ['error', 'always'],
               'quotes': ['error', 'single'],
            },
         },
      ];
      ```
   - Make sure you have a `tsconfig.json` in your project root.

---

## Resources

- [ESLint Flat Config Guide](https://eslint.org/docs/latest/use/configure/configuration-files-new)
- [TypeScript ESLint Docs](https://typescript-eslint.io/)
- [Prettier Docs](https://prettier.io/docs/en/index.html)
- [Husky Docs](https://typicode.github.io/husky/#/)
