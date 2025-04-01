---
created-date: 2025-02-20 10:40
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
  - Installing Python
status: Completed
---

#  Installing Python

## Introduction
- As a TypeScript developer, you're used to installing Node.js as your runtime environment
- The problem: Python's installation process differs significantly across operating systems and has several pitfalls that Node.js doesn't
- In this lesson, you'll learn: 
	- (1) how to install Python properly on any platform
	- (2) how to verify your installation is working correctly
	- (3) how to navigate common Python environment issues
- After this video, you'll have a working Python environment set up the right way, avoiding the common mistakes that plague new Python developers

## Main Content

### A. Core Concept Comparison

#### Environment Installation: Node.js vs Python

```typescript
// Node.js/TypeScript ecosystem
// One main runtime to install (Node.js)
// npm comes bundled with Node.js
// Global path usually set automatically
```

```python
# Python ecosystem
# Python runtime needs to be installed
# pip (package manager) comes with Python 3.4+
# PATH configuration sometimes manual
```

When moving from TypeScript to Python, think of the Python interpreter as similar to Node.js - it's the runtime that executes your code. However, Python's installation process has more nuances across different platforms.

### B. Installation by Operating System

>[Download Python | Python.org](https://www.python.org/downloads/)

1. Windows Installation
```bash
# TypeScript/Node.js way
# Download installer from nodejs.org and run it

# Python way 
# 1. Download installer from python.org 
# 2. CRITICAL: Check "Add Python to PATH" during installation 
# 3. Also select "Install pip" (usually selected by default) 
# 4. Verify installation:
python --version
# or
python3 --version # Windows Python launcher
```

>[!info] Windows Python Launcher: Windows offers a special `py` command which helps manage multiple Python versions. Use `py -3` to ensure Python 3 is used.

2. macOS Installation
```bash
# TypeScript/Node.js way
brew install node

# Python way
# Option 1: Homebrew (recommended)
brew install python  # Installs Python 3

# Option 2: Official installer from python.org
# Download and run the macOS installer

# Option 3: Use the pre-installed Python (not recommended)
# macOS comes with Python, but it might be Python 2.7
# and is intended for system use
```

>[!warning] macOS comes with a system Python (often 2.7) but you should avoid using it for development. Install a separate Python 3 version.

3. Linux Installation
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install python3 python3-pip python3-venv

# Fedora
sudo dnf install python3 python3-pip python3-virtualenv

# Arch Linux
sudo pacman -S python python-pip
```

### C. Checking Installation

##### 1. Checking Python Version and Location

```bash
# Check Python version (should be 3.x)
python --version    # or python3 --version

# Find where Python is installed
# On Windows:
where python
# On macOS/Linux:
which python
which python3

# Check if pip is installed
pip --version    # or pip3 --version
```

>[!info] `pip` is Python's package manager. More on this in upcoming videos

##### 2. Common Installation Issues and Solutions

```bash
# Issue 1: 'python' is not recognized as a command
# Solution on Windows: 
# - Reinstall and check "Add Python to PATH"
# - Or use full path: C:\Python39\python.exe
# - Or use py launcher: py -3

# Issue 2: Both Python 2 and 3 are installed
# Check which version each command uses:
python --version   # Might be Python 2
python3 --version  # Python 3

# Issue 3: pip not working
# Install pip if missing:
# Download get-pip.py from https://bootstrap.pypa.io/get-pip.py
python get-pip.py
```


### D. Practical Implementation

#### 1. Creating and Running Your First Python Script

```python
# Create a file named hello.py
# hello.py
print("Hello from Python!")

# Import statement example
import sys
print(f"Using Python {sys.version_info.major}.{sys.version_info.minor}.{sys.version_info.micro}")
print(f"Python executable path: {sys.executable}")
```

```bash
# Run the script
python hello.py
# or
python3 hello.py
# Windows alternative
py hello.py
```

#### 2. Updating Python and Managing Multiple Versions

```bash
# Updating Python
# Unlike npm update -g npm, Python requires reinstallation for updates

# Windows: Download new installer
# macOS: brew upgrade python
# Linux: Use package manager (apt, dnf, etc.)

# For multiple Python versions:
# Windows: Use py launcher with version flag
py -3.9 --version  # Use Python 3.9
py -3.10 --version # Use Python 3.10

# macOS/Linux: Consider pyenv (similar to nvm for Node.js)
# Install pyenv (example for macOS):
brew install pyenv

# Install specific Python version:
pyenv install 3.9.13
pyenv install 3.10.4

# Set global Python version:
pyenv global 3.10.4
```

## Summary
* Key points to remember:
  - Windows: Check "Add Python to PATH"
  - macOS/Linux: May already have Python installed
  - Always verify the version after installation
  - Use `python3` command if both Python 2 and 3 are installed


### Pro Tips
* On Windows, consider using Windows Subsystem for Linux (WSL) for a Unix-like environment
* Modern IDEs like VS Code will detect your Python installation automatically
* Python's installer on Windows can fix PATH issues with 'Repair' option

## Resources
- [python.org/downloads/windows](https://www.python.org/downloads/windows)
- [python.org/downloads/macos](https://www.python.org/downloads/macos)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```