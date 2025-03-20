---
created-date: 2025-02-20 19:25
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
  - Virtual Environments
  - Node Version Manager
status: Completed
---

# Environment Isolation: venv vs nvm

> In TypeScript, your packages go to a folder in your project, and you switch Node versions globally. In Python, you step into a complete isolated environment that contains both Python and packages.

## Introduction

Both TypeScript and Python need environment management, but their approaches differ fundamentally. In this lesson, you'll learn:

- How Python's virtual environments compare to Node.js version management
- Why Python combines package and version management in a single solution
- How to create, activate, and use Python virtual environments
- Common pitfalls when switching between multiple Python projects
- Modern tools that provide more npm-like experiences in the Python ecosystem

## Core Concept Differences

> **Key Insight**: Node.js splits version and package management into separate tools, while Python combines them in a single solution.

The fundamental difference between Node.js and Python environments lies in their approach to isolation. Understanding these differences is crucial for effective project management in Python.

### Node.js/TypeScript Approach

Node.js separates concerns between version management and package management:

- **nvm**: Manages different Node.js versions
- **node_modules**: Handles package isolation per project
- Two separate tools handle different aspects of environment management

#### What happens in practice:

```bash
# Step 1: Switch Node version globally
nvm use 16.14.0    # Affects ALL projects on your machine!

# Step 2: Install packages locally
npm install        # Goes to ./node_modules in current project
```

> [!warning] When switching Node versions through nvm, old node_modules might become incompatible and cause difficult-to-debug dependency issues. The fix often requires running `rm -fR node_modules && npm i`.

### Python Approach

Python combines version and package management in one solution:

- **venv**: Creates a complete isolated environment just for your project
- Contains both Python interpreter and packages
- Single tool manages both interpreter and packages

#### What happens in practice:

```bash
# Step 1: Create an isolated environment
python -m venv venv

# Step 2: Step INTO that environment
source venv/bin/activate    # You're now in a different world

# Step 3: Install packages in that environment
pip install flask           # Goes to the activated environment
```

This approach is generally safer than Node's split approach, as it prevents dependency conflicts between different projects and avoids global changes.

## Transition Path for TypeScript Developers

> **Quick Start**: If you're a TypeScript developer wanting the most familiar experience, jump to the "Modern Tools" section and try Poetry, which provides an npm-like workflow.

Here's how to shift your thinking when moving from TypeScript to Python development:

|TypeScript Habit|Python Equivalent|Mental Shift Needed|
|---|---|---|
|`nvm use 16` globally|`source venv/bin/activate` per project|Think "entering" not "switching"|
|`npm install` in project|`pip install` in activated env|Check which env is active before installing|
|`node_modules` in project|`site-packages` in venv|Packages live with the interpreter, not in your project|
|Package.json tracking|requirements.txt tracking|Manually export your dependencies|

## Basic Operations

### Node.js Environment Management

```bash
# Installing Node versions
nvm install 16.14.0
nvm install 18.0.0

# Switching versions
nvm use 16.14.0
nvm use 18.0.0

# Package isolation happens automatically via node_modules
npm install express  # Goes to ./node_modules
```

### Python Virtual Environments

#### Setting Up a New Project

```bash
# Creating virtual environment
python -m venv venv  # Standard name is 'venv'

# Activating virtual environment
source venv/bin/activate         # Unix/macOS
venv\Scripts\activate.bat        # Windows Command Prompt
venv\Scripts\Activate.ps1        # Windows PowerShell

# Deactivating when done
deactivate
```

#### Working with Existing Project

```bash
# Save currently used packages
pip freeze > requirements.txt

# Create new environment
python -m venv venv

# Activate
source venv/bin/activate         # Unix/macOS
venv\Scripts\activate.bat        # Windows Command Prompt
venv\Scripts\Activate.ps1        # Windows PowerShell

# Install dependencies
pip install -r requirements.txt
```

## Modern Tools for TypeScript Developers

> **TypeScript-Friendly Tools**: These modern Python tools provide a more familiar experience if you're coming from npm/yarn.

If the standard `venv` workflow feels too different, these tools offer a more npm-like experience:

### Poetry (Recommended for TS Developers)

Poetry is the closest thing Python has to npm/yarn. It handles dependencies, environments, and packaging in a way that will feel familiar:

```bash
# Install Poetry
curl -sSL https://install.python-poetry.org | python3 -

# Create new project (like npm init)
poetry new my-project
cd my-project

# Activate environment (creates and manages venv for you)
poetry shell

# Add dependencies (like npm install --save)
poetry add flask

# Add dev dependencies (like npm install --save-dev)
poetry add pytest --dev

# Install from poetry.lock (like npm ci)
poetry install

# Update dependencies (like npm update)
poetry update
```

#### Key benefits for TypeScript developers:

- Lockfile for reproducible builds (like package-lock.json)
- Clear dev vs. production dependencies
- Version resolution like npm
- No need to manually manage venv creation and activation

### Pipenv (Official Python Packaging Tool)

Similar to Poetry but more established:

```bash
# Install Pipenv
pip install --user pipenv

# Create environment & install dependencies
pipenv install
pipenv install pytest --dev

# Activate environment
pipenv shell
```

## Key Differences in Practice

> **Common Mistake**: Forgetting to activate your virtual environment before installing packages

Understanding these practical differences is crucial for TypeScript developers, as Python's approach to environment management requires more explicit actions and awareness of the current state.

### Environment Activation

#### Node.js/TypeScript

```bash
# Just switches Node version
nvm use 22.7.0              

# Package context from location
cd my-project               
```

#### Python

```bash
# Must explicitly activate
source myproject_env/bin/activate  

# All commands now use this environment
pip install flask  # Goes to active environment
```

### Package Installation Context

#### Node.js/TypeScript

```bash
# Always installs to current directory's node_modules
npm install express  # Goes to ./node_modules
```

#### Python

```bash
# Installs to active virtual environment
pip install flask   # Goes to active venv or global if no venv
```

> [!danger] The most common mistake TypeScript developers make is forgetting to activate their virtual environment before installing packages. Always check your prompt for `(venv)` before running pip!

### Project Isolation Structure

#### Node.js/TypeScript project

```
my-node-project/
├── node_modules/     # Local packages
├── package.json
└── src/
```

#### Python project

```
my-python-project/
├── venv/           # Complete environment
│   ├── bin/        # Python interpreter & tools
│   ├── lib/        # Installed packages
│   └── pyvenv.cfg  # Environment config
├── requirements.txt
└── src/
```

### Visual Comparison: Environment Isolation

```text
┌─────────────────────────────────────┐     ┌───────────────────────────────┐
│           Node.js/TypeScript        │     │                Python                │
└─────────────────────────────────────┘     └───────────────────────────────┘
         ▲                   ▲                           ▲
         │                   │                           │
┌────────┴─────────┐ ┌───────┴────────┐       ┌──────────┴─────────┐
│       nvm        │ │  node_modules  │       │        venv        │
│                  │ │                │       │                    │
│ ┌──────────────┐ │ │ ┌────────────┐ │       │ ┌────────────────┐ │
│ │ Node v16.14.0│ │ │ │ Package A  │ │       │ │ Python 3.9     │ │
│ └──────────────┘ │ │ └────────────┘ │       │ └────────────────┘ │
│ ┌──────────────┐ │ │ ┌────────────┐ │       │                    │
│ │ Node v18.0.0 │ │ │ │ Package B  │ │       │ ┌────────────────┐ │
│ └──────────────┘ │ │ └────────────┘ │       │ │ Package A      │ │
│                  │ │                │       │ └────────────────┘ │
│   Version Mgmt   │ │ Package Mgmt   │       │ ┌────────────────┐ │
└──────────────────┘ └────────────────┘       │ │ Package B      │ │
                                              │ └────────────────┘ │
                                              │                    │
                                              │  All-in-one Mgmt   │
                                              └────────────────────┘
```

## Real-World Workflow Example

Here's a day-in-the-life comparison of working with both ecosystems on multiple projects:

### TypeScript Workflow

```bash
# Morning: Work on Project A
cd ~/projects/project-a
nvm use 22.7.0
npm install new-dependency
npm start

# Afternoon: Switch to Project B
cd ~/projects/project-b
nvm use 20.7.0  # Different Node version
npm install different-dependency
npm start
```

### Python Workflow

```bash
# Morning: Work on Project A
cd ~/projects/project-a
source venv/bin/activate  # Prompt changes to (venv)
pip install new-dependency
python app.py

# Afternoon: Switch to Project B
deactivate  # Exit Project A's environment
cd ~/projects/project-b
source venv/bin/activate  # Enter Project B's environment
pip install different-dependency
python app.py
```

The key difference is that Python requires explicit environment activation/deactivation, while Node.js implicitly uses the local node_modules.

## Common Pitfalls and Troubleshooting

> **Environment Verification**: The single most important habit is to always verify which environment you're in before running commands.

### 1. "No module named X" Error

**Problem**: You installed a package but Python can't find it **Solution**: Check if your virtual environment is active

```bash
# Verify active environment
which python  # Should point to venv
pip list      # Check installed packages
```

**What happened?** You likely installed the package in:

1. A different virtual environment than the one you're running in
2. The global Python environment while your venv is active
3. Your virtual environment, but you're running Python globally

### 2. Permission Errors

**Problem**: Unable to install packages due to permission issues **Solution**: You're likely trying to install to global Python without admin rights

```bash
# Wrong (without venv):
sudo pip install flask  # Don't do this!

# Right (with venv):
source venv/bin/activate
pip install flask       # No sudo needed
```

**What happened?** Installing to the global Python requires admin privileges. Virtual environments are user-owned, so no special permissions are needed.

### 3. Environment Not Activating

**Problem**: Virtual environment commands not found or not working **Solution**: Check environment creation and activation method

```bash
# Recreate environment if needed
rm -rf venv
python -m venv venv

# Ensure correct activation for your shell
source venv/bin/activate  # Bash/Zsh
. venv/bin/activate       # Some shells need this syntax
```

**What happened?** The environment might be corrupted, or you might be using the wrong activation command for your shell.

### 4. Package Version Conflicts

**Problem**: Package installations fail with version conflicts **Solution**: Use a separate environment for each project

```bash
# Don't share environments between projects
# Instead, create a new one for each project:
python -m venv project1_env
python -m venv project2_env
```

### 5. Forgetting Which Environment You're In

**Problem**: Lost track of which environment is active **Solution**: Check your command prompt and use environment inspection commands

```bash
# Check prompt for environment name
(venv) $  # You're in the "venv" environment

# Check Python path
which python
/path/to/venv/bin/python  # You're in a venv
/usr/bin/python  # You're using global Python
```

## Best Practices

Following these practices will help you avoid common issues and maintain clean, reproducible Python development environments. They're especially important as Python doesn't have the automatic isolation that Node.js provides with node_modules.

### Environment Creation

```bash
# Create venv in project directory
python -m venv .venv  # Hidden directory (recommended)
# OR
python -m venv venv   # Visible directory
```

### Environment Activation Check

```bash
# Check if in correct Node version
node --version

# Check if in Python venv
which python  # Unix/macOS
where python  # Windows
```

### Project Setup Recommendations

- Always create venv when starting new Python project
- Add venv directory to .gitignore
- Create requirements.txt or use modern tools
- Document Python version requirements

### Different Python Versions

If you need to work with multiple Python versions, consider using a dedicated version manager like pyenv:

```bash
# Install pyenv (macOS with Homebrew)
brew install pyenv

# List available Python versions
pyenv install --list

# Install specific Python version
pyenv install 3.9.0

# Set global Python version
pyenv global 3.9.0

# Set local project Python version
pyenv local 3.8.5
```

Then combine pyenv with venv for complete isolation:

```bash
# Use pyenv to select Python version
pyenv local 3.9.0

# Create venv using that Python version
python -m venv venv
```

## Conda as an Alternative to venv

> **Data Science Choice**: If you'll be doing data science or ML work, consider Conda instead of venv.

Alongside venv, there's a popular solution for managing environments - Conda, particularly common in data science, machine learning, and data analysis projects.

### What is Conda?

Conda is a versatile package and environment manager that:

- Manages both Python packages and packages outside the Python ecosystem
- Handles libraries and dependencies written in C/C++ without needing to compile them
- Works cross-platform (Windows, macOS, Linux)
- Offers its own package repository (conda-forge)

### Basic Conda Operations

```bash
# Installing Conda (via Miniconda or Anaconda)
# https://docs.conda.io/en/latest/miniconda.html

# Creating an environment
conda create --name myenv python=3.9

# Activating the environment
conda activate myenv

# Installing packages
conda install numpy pandas matplotlib

# Exporting environment
conda env export > environment.yml

# Importing environment
conda env create -f environment.yml

# Deactivation
conda deactivate
```

### Conda vs venv

|Aspect|venv|Conda|
|---|---|---|
|Python version management|No (requires pyenv)|Yes|
|Non-Python packages|No|Yes|
|Installation speed|Slower (compilation)|Faster (pre-compiled)|
|Binary dependency management|Limited|Advanced|
|Environment size|Smaller|Larger|
|Ideal for|Standard projects|Data Science, ML|

> [!info] Conda is very often used along with `Jupyter Notebooks`. More on that in one of the next lectures.

## Environment Variables and Configuration

In Python projects, environment variables are often managed differently than in Node.js:

### Node.js Approach

```bash
# Using dotenv package
npm install dotenv
```

```js
// index.js
require('dotenv').config();
const apiKey = process.env.API_KEY;
```

### Python Approach

```bash
# Using python-dotenv package
pip install python-dotenv
```

```python
# main.py
import os
from dotenv import load_dotenv

load_dotenv()  # Load .env file
api_key = os.environ.get("API_KEY")
```

> Both ecosystems use `.env` files, but the usage patterns differ slightly.

## Tips for TypeScript Developers

> **Essential Habit**: Always check your terminal prompt for the `(venv)` prefix before running pip commands.

- Think of venv as stepping into a completely different machine
- Always check for the `(venv)` prefix in your terminal
- Never install Python packages globally (avoid `sudo pip install`)
- Use one venv per project - don't try to share environments
- Consider Poetry for the most npm-like experience
- Add `venv/` and `__pycache__/` to your .gitignore
- If deploying, test in a fresh environment to verify all deps are captured
- Remember to deactivate one environment before activating another

## Quick Reference: Environment Commands

|Task|Node.js/TypeScript|Python|
|---|---|---|
|Install version manager|`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh \| bash`|`pip install pyenv`|
|Install runtime version|`nvm install 16.14.0`|`pyenv install 3.9.0`|
|Switch version globally|`nvm use 16.14.0`|`pyenv global 3.9.0`|
|Create project environment|N/A (uses node_modules)|`python -m venv venv`|
|Activate environment|N/A|`source venv/bin/activate`|
|Install dependencies|`npm install`|`pip install -r requirements.txt`|
|Add dependency|`npm install express`|`pip install flask`|
|Save dependencies|`npm init` (auto)|`pip freeze > requirements.txt`|

## Resources
- [  ] 

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```