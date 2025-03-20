---
created-date: 2025-02-20 19:15
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/2
aliases: 
summary:
  - Python for TS devs 1
  - requirements.txt vs package.json
  - Package Management
status: Completed
---

# Package Management: pip vs npm/yarn

> In TypeScript, packages get listed in package.json and automatically installed to node_modules. In Python, you manually install packages with pip and must separately track them in requirements.txt.

## Introduction

Both ecosystems have package managers, but they follow different philosophies. In this lesson, you'll learn:

- How pip differs from npm/yarn in its fundamental approach to package installation
- The structure and purpose of requirements.txt compared to package.json
- Different strategies for version pinning and dependency resolution
- Common Python dependency management pitfalls and how to avoid them
- Modern alternatives like Poetry and Pipenv that offer npm-like experiences

## Basic Operations Comparison

> **Key Insight**: In *npm*, packages are automatically tracked. In *pip*, you install first, then manually track what you installed.

### Installation Commands

```bash
# npm/yarn                          | pip
npm install lodash                  | pip install requests      # Goes to active environment
npm install -g typescript           | pip install --user pylint # Installs to user's home
yarn add express                    | pip install flask         # Goes to active environment
npm i axios@1.4.0                   | pip install pandas==2.0.0  # Specific version
```

**Important Note**: Python's approach is fundamentally different:

- Installation location depends on the active environment
- Global installations should be avoided (use virtual environments) - _more on this in the previous lecture_
- `--user` flag installs to user's home directory, but isn't recommended for project dependencies

## Dependency File Comparison

> **Remember**: In Python, you must manually keep requirements.txt in sync with your installed packages - it doesn't update automatically like package.json!

### Basic Structure Comparison

**package.json (Node.js/TypeScript)**

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "Project description",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.17.1",
    "lodash": "~4.17.21",
    "axios": "1.4.0"
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "typescript": "^5.0.0"
  }
}
```

**requirements.txt (Python)**

```
# Main dependencies
flask==2.0.1
requests==2.31.0
pandas==2.0.0

# Optional: add comments for clarity
# API framework
fastapi==0.100.0

# Database
sqlalchemy==2.0.0
```

## Real-World Workflow Comparison

Here's how adding a new dependency works in each ecosystem:

### TypeScript Workflow

```bash
# 1. Install the package
npm install axios

# 2. That's it! package.json was automatically updated
```

### Python Workflow

```bash
# 1. Install the package
pip install requests

# 2. Manually update requirements.txt
pip freeze > requirements.txt  # Captures ALL installed packages

# Or add it manually to requirements.txt
echo "requests==2.31.0" >> requirements.txt
```

The key difference is that Python requires explicit tracking of dependencies - they aren't automatically recorded when installed.

## Key Differences

### 1. File Purpose

**package.json**:

- Project metadata
- Dependencies
- Scripts
- Project configuration

**requirements.txt**:

- Dependencies only
- No project metadata
- No scripts or configuration
- Just a list of packages and versions

### 2. Version Specification

```python
# Node.js (package.json)        | Python (requirements.txt)
"express": "^4.17.1"            | flask==2.0.1    # Exact version
"lodash": "~4.17.21"            | pandas>=2.0.0   # Minimum version
"axios": "1.4.0"                | requests~=2.31.0 # Compatible release
```

**Version operators comparison**:

|Node.js|Meaning|Python|Meaning|
|---|---|---|---|
|`^` (caret)|Minor updates|`~=`|Compatible release (updates up to the next significant release)|
|`~` (tilde)|Patch updates|`>=`|Minimum version|
|`>`|Greater than|`>`|Greater than|
|`>=`|Minimum version|`>=`|Minimum version|
|`<=`|Maximum version|`<=`|Maximum version|
|`=`|Exact version|==|Exact version|
|||`!=`|Not equal to|

**Note about Python's `~=` operator**: `requests~=2.31.0` means "compatible with 2.31.0" - any version >= 2.31.0 but < 2.32.0. It allows patch updates but not minor or major updates.

### 3. Dependency Types

```
# Node.js                      | Python
"dependencies": {}             | requirements.txt
"devDependencies": {}          | requirements-dev.txt (convention)
"peerDependencies": {}         | (no equivalent)
```

In Python, there's no built-in concept of development dependencies - developers typically create separate requirements files by convention.

## Dependency Resolution

> **Warning**: Python's dependency resolution is more prone to conflicts than Node.js's nested approach.

One significant difference between *pip* and *npm* is how they handle dependency resolution:

**npm/yarn**:

- Uses a complex dependency resolution algorithm
- Creates a dependency tree (package-lock.json)
- Handles conflicting dependencies through nested node_modules
- Automatic deduplication of dependencies

**pip**:

- Simpler, more linear resolution
- No built-in lock file (though pip-tools provides this)
- Can struggle with conflicting dependencies
- No automatic deduplication

This difference can lead to "dependency hell" situations in Python that are less common in Node.js.

### Dependency Hell Explained

"Dependency hell" in the Python context manifests itself through several characteristic problems:

1. **Version conflicts** - When two packages require different, incompatible versions of the same library. For example, package A requires `requests==2.22.0`, while package B requires `requests>=2.24.0`. In such cases, pip simply installs the last requested version, which can lead to unexpected errors.
    
2. **Transitive dependency resolution** - pip doesn't handle long dependency chains well. When package A depends on B, which depends on C, which requires a specific version of D, problems that are difficult to diagnose may arise.
    
3. **Lack of a default lock mechanism** - without tools like pip-tools, it's difficult to ensure identical environments across different machines, leading to the classic "works on my machine" problem.
    
4. **Downgrade problems** - pip often can't automatically downgrade an already installed package when required by a new dependency.
    

In contrast, the Node.js ecosystem with *npm/yarn* handles these problems better thanks to the nested node_modules structure, which allows different versions of the same library to coexist, and the detailed recording of exact versions in package-lock.json, which greatly facilitates environment reproducibility.

## Common Patterns

### Multiple Requirements Files

Python projects often split requirements into multiple files:

```
requirements/
├── base.txt         # Core dependencies
├── development.txt  # Development tools
├── production.txt   # Production-specific
└── test.txt        # Testing dependencies
requirements.txt     # Default one
```

**Usage**:

```bash
# Development
pip install -r requirements/development.txt

# Production
pip install -r requirements/production.txt

# Multiple configurations
pip install -r requirements/base.txt -r requirements/test.txt
```

A common pattern is to have the specialized requirements files import from the base file:

```python
# Inside requirements/development.txt
-r base.txt

# Development-specific packages
pytest==7.4.0
black==23.7.0
```

This structure achieves what package.json does with dependencies/devDependencies, but requires manual maintenance.

### Private Packages and Repositories

**Node.js**:

```
// .npmrc
@mycompany:registry=https://npm.mycompany.com/
//npm.mycompany.com/:_authToken=${NPM_TOKEN}
```

**Python**:

```
# pip.conf or using command line
pip install --index-url https://pypi.mycompany.com/simple/ my-private-package
```

Or in requirements.txt:

```
--index-url https://pypi.mycompany.com/simple/
my-private-package==1.0.0
```

## Modern Alternatives

> **TypeScript Developer Tip**: If pip's manual approach feels cumbersome, you'll find Poetry much more familiar.

### Poetry (pyproject.toml)

Poetry provides an npm-like experience:

- Automatic dependency tracking
- Lock file for reproducibility
- Dev vs. regular dependencies
- Scripts similar to npm scripts

```toml
[tool.poetry]
name = "my-project"
version = "1.0.0"
description = "Project description"

[tool.poetry.dependencies]
python = "^3.9"
requests = "^2.31.0"

[tool.poetry.dev-dependencies]
pytest = "^7.4.0"
```

Installing with Poetry:

```bash
# Add package (like npm install --save)
poetry add requests

# Add dev package (like npm install --save-dev)
poetry add pytest --dev

# Install from lock file (like npm ci)
poetry install
```

### Pipenv (Pipfile)

```
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
requests = "==2.31.0"
flask = ">=2.0.0"

[dev-packages]
pytest = "*"
```

Installing with Pipenv:

```bash
# Add package
pipenv install requests

# Add dev package
pipenv install pytest --dev

# Install from lock file
pipenv install
```

### pip-tools

An intermediate solution that enhances pip while maintaining its simplicity:

```bash
# Install pip-tools
pip install pip-tools

# Create requirements.in file
echo "requests>=2.30.0" > requirements.in

# Generate requirements.txt from requirements.in
pip-compile requirements.in

# Install exactly the versions specified
pip-sync requirements.txt
```

>`requirements.in` is a file used internally by `pip-tools` for depenency management

pip-tools gives you a lock file without switching to a completely different system.

## Wheels vs Source Distributions

Python packages come in two main formats:

**Wheels (.whl files)**:

- Pre-built binary packages
- Faster to install
- No compilation needed
- Similar to npm's pre-compiled packages

**Source distributions (.tar.gz files)**:

- Need to be compiled during installation
- Can cause issues if compilation tools are missing
- More flexible for different platforms

pip tries to install wheels when available, falling back to source distributions when needed.

## Best Practices

### 1. Version Pinning

```python
# Good - Exact versions
flask==2.0.1
requests==2.31.0

# Avoid - Unpinned versions
flask
requests>=2.0.0
```

Pinning exact versions ensures reproducible builds and prevents unexpected updates.

### 2. Dependency Organization

```python
# Group related dependencies
# Web framework
flask==2.0.1
werkzeug==2.0.0

# Database
sqlalchemy==2.0.0
psycopg2-binary==2.9.1
```

Organizing dependencies by purpose improves readability and maintenance.

### 3. Comments and Documentation

```python
# API dependencies
fastapi==0.100.0  # REST API framework
uvicorn==0.22.0   # ASGI server

# Add notes about specific versions if needed
tensorflow==2.13.0  # Requires CUDA 11.8
```

Clear comments help team members understand why specific dependencies are included.

## Common Pitfalls

> **Most Common Mistake**: Installing packages without updating requirements.txt, then wondering why it works locally but not elsewhere.

### 1. Unpinned Dependencies

**Problem**: Using `requests` or `requests>=2.0.0` in requirements.txt **Result**: Different environments get different versions **Solution**: Always pin exact versions (`requests==2.31.0`)

### 2. Missing Dependencies

**Problem**: Installing with pip but forgetting to update requirements.txt **Result**: "Works on my machine" syndrome **Solution**: Use `pip freeze > requirements.txt` or better, use Poetry

### 3. Development Dependencies

**Problem**: No built-in separation between dev and prod dependencies **Result**: Production environments get testing tools and other unnecessary packages **Solution**: Use multiple requirements files or switch to Poetry/Pipenv

### 4. Dependency Conflicts

**Problem**: Two packages require incompatible versions of a dependency **Result**: Unpredictable behavior as pip installs the last requested version **Solution**: Use virtual environments, be careful with version ranges, consider pip-tools

## Quick Reference: Package Management Commands

|Task|npm/yarn|pip|
|---|---|---|
|Install package|`npm install lodash`|`pip install requests`|
|Install specific version|`npm i axios@1.4.0`|`pip install pandas==2.0.0`|
|Install from file|`npm install`|`pip install -r requirements.txt`|
|Update package|`npm update lodash`|`pip install -U requests`|
|List packages|`npm list`|`pip list`|
|Remove package|`npm uninstall lodash`|`pip uninstall requests`|
|Save dependencies|Auto-saved to package.json|`pip freeze > requirements.txt`|
|Global installation|`npm install -g typescript`|`pip install --user black`|

## Tips for TypeScript Developers

### General Approach:

- Remember that pip doesn't automatically track what you install
- Run `pip freeze > requirements.txt` after installing new packages
- Use modern tools (Poetry/Pipenv) for more npm-like experience
- Always pin your versions for reproducibility
- Consider splitting requirements for different environments

### Version Management:

- Be explicit with versions - Python dependency resolution is less forgiving
- Use virtual environments (covered in previous lesson)
- Regularly update requirements.txt
- Use `pip freeze` to capture the exact installed versions

### Best Practices:

- Document dependency groups with comments
- Maintain separate requirements files for dev and prod
- Use pip-tools for better dependency management
- Always test dependency changes in a clean environment

### Transition Tips:

- Start with basic requirements.txt
- Move to Poetry/Pipenv when comfortable
- Always use version control
- Document project setup process

## Case Study: Adding a New Feature

Let's look at the complete workflow when adding a feature that requires a new dependency:

### TypeScript Approach

```bash
# 1. Start a feature branch
git checkout -b feature/add-api-client

# 2. Install the dependency
npm install axios

# 3. Write code using the dependency
# package.json was automatically updated

# 4. Commit and push
git add package.json package-lock.json src/
git commit -m "Add API client using axios"
git push
```

### Python Approach

```bash
# 1. Start a feature branch
git checkout -b feature/add-api-client

# 2. Install the dependency
pip install requests

# 3. Write code using the dependency

# 4. Update requirements.txt
pip freeze > requirements.txt
# Or manually add: echo "requests==2.31.0" >> requirements.txt

# 5. Commit and push
git add requirements.txt src/
git commit -m "Add API client using requests"
git push
```

The key difference is step 4 - manually updating requirements.txt, which is not needed in the TypeScript approach.


## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```