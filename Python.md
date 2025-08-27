# Python

## Table of Contents
- [Python](#python)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Initial Setup](#initial-setup)
    - [Flake8](#flake8)
    - [Black](#black)
    - [Ignore Files](#ignore-files)
      - [`.gitignore`](#gitignore)
      - [`.flake8` (exclude section)](#flake8-exclude-section)
    - [Pre-commit Git Hooks](#pre-commit-git-hooks)
  - [Scripts](#scripts)
  - [Usage](#usage)
  - [Resources](#resources)

---

## Introduction

Maintaining code quality is crucial for any Python project. This guide covers:
- **Flake8**: Checks for style issues and potential bugs, making your code more readable and reliable.
- **Black**: Automatically formats your code, ensuring consistency and reducing time spent on style debates.
- **Pre-commit**: Runs checks before commits, preventing bad code from entering your repository.

These tools help teams focus on building features, not fixing preventable bugs or debating style.

---

## Initial Setup

### Flake8

**Why Flake8?**
Flake8 analyzes your Python code for style issues and potential bugs. It enforces standards and helps catch errors early, improving code reliability and maintainability.

1. Install Flake8:
   ```bash
   pip install flake8
   ```
2. Create a `.flake8` config file in your project root:
   ```ini
   [flake8]
   max-line-length = 88
   exclude = .git,__pycache__,build,dist
   ```
3. Run Flake8:
   ```bash
   flake8 .
   ```

### Black

**Why Black?**
Black automatically formats your Python code, ensuring a consistent style across your team. This reduces time spent on formatting debates and makes code reviews more focused on logic.

1. Install Black:
   ```bash
   pip install black
   ```
2. Run Black:
   ```bash
   black .
   ```

### Ignore Files

**Why Ignore Files?**
Ignore files prevent unnecessary or generated files (like dependencies and build artifacts) from being linted or formatted, speeding up checks and avoiding false positives.

#### `.gitignore`
```
__pycache__/
build/
dist/
*.pyc
*.pyo
*.egg-info/
.env
.venv
```

#### `.flake8` (exclude section)
```
exclude = .git,__pycache__,build,dist
```

### Pre-commit Git Hooks

**Why Pre-commit?**
Pre-commit automates checks before commits, ensuring that only code passing lint and format checks makes it into your repository. This protects your main branch from preventable errors and enforces team standards automatically.

1. Install pre-commit:
   ```bash
   pip install pre-commit
   ```
2. Create a `.pre-commit-config.yaml` in your project root:
   ```yaml
   repos:
     - repo: https://github.com/psf/black
       rev: stable
       hooks:
         - id: black
     - repo: https://github.com/pycqa/flake8
       rev: stable
       hooks:
         - id: flake8
   ```
3. Install the git hook:
   ```bash
   pre-commit install
   ```

---

## Scripts

Add these scripts to your workflow for easy linting and formatting:

- Format code:
  ```bash
  black .
  ```
- Lint code:
  ```bash
  flake8 .
  ```

---

## Usage

1. Write code as usual.
2. Run `black .` to format code.
3. Run `flake8 .` to check for lint errors.
4. Commit your changes—pre-commit will run the hooks automatically.

---

## Resources

- [Flake8 Docs](https://flake8.pycqa.org/en/latest/)
- [Black Docs](https://black.readthedocs.io/en/stable/)
- [Pre-commit Docs](https://pre-commit.com/)
