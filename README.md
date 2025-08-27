# CSES-Linters-Formatters-Hooks

## Linters, Formatters, and CI/CD with Husky

## 1. Introduction  
In modern development, three tools form the backbone of code quality:  

- **Linters**: Analyze code for errors, anti-patterns, and maintainability.  
- **Formatters**: Keep code style consistent across teams.  
- **Git hooks / CI/CD**: Automate checks before committing and deploying.  

These tools together create a development environment where developers can focus on logic instead of style disputes or late bug catching.

---

# Linters, Formatters, and Git Hooks

## Table of Contents
- [CSES-Linters-Formatters-Hooks](#cses-linters-formatters-hooks)
  - [Linters, Formatters, and CI/CD with Husky](#linters-formatters-and-cicd-with-husky)
  - [1. Introduction](#1-introduction)
- [Linters, Formatters, and Git Hooks](#linters-formatters-and-git-hooks)
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
  - [Resources](#resources)

---

## Introduction

This repo demonstrates a standard setup for code quality tools in JavaScript/TypeScript projects:
- **ESLint** for linting (using Flat Config)
- **Prettier** for formatting
- **Husky** for Git hooks

---

## Initial Setup

### ESLint (Flat Config)

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

1. Install Prettier:
	```bash
	npm install --save-dev prettier
	```
2. Create `.prettierrc`:
	```json
	{
	  "semi": true,
	  "singleQuote": true
	}
	```

### Ignore Files

Add these files to your project root:

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

## Resources

- [ESLint Flat Config Guide](https://eslint.org/docs/latest/use/configure/configuration-files-new)
- [Prettier Docs](https://prettier.io/docs/en/index.html)
- [Husky Docs](https://typicode.github.io/husky/#/)
