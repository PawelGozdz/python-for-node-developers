---
created-date: 2025-02-20 19:07
tags:
  - investment
  - udemy
  - tech/nodejs
  - tech/python
  - udemy/course
  - course/python-for-ts-devs
  - section/3
aliases: 
summary:
  - Python for TS devs 1
  - Project Structure
status: Completed
---

# Python vs Node.js Project Structure

## Introduction

Both TypeScript and Python projects need organized structure, but they follow different conventions. In this lesson, you'll learn:

- How Python's project organization differs from TypeScript's compilation model
- The key directories and files needed in a standard Python project
- Naming conventions and configuration approaches that Python developers follow
- How Python's package system influences project structure
- Practical strategies for organizing your Python code coming from a TypeScript background

## Guide for TypeScript Developers

When transitioning from TypeScript to Python, understanding project structure differences is crucial. While both ecosystems share similar concepts, their implementation and organization patterns differ significantly. This guide will help you understand these differences and adapt your development practices accordingly.

## 1. Basic Project Layout & Organization

One of the fundamental differences between Python and Node.js projects lies in how they organize code and manage project structure. Understanding these differences will help you navigate Python projects more effectively.

### Standard Project Layouts

```bash
# Node.js/TypeScript Project
my-node-project/
├── node_modules/     # Third-party dependencies
├── src/             # Source code root
├── dist/            # Compiled code
├── tests/           # Test files
└── scripts/         # Build/deployment scripts
```

The Node.js structure is designed around the npm ecosystem and TypeScript compilation process. It separates source code from compiled output and keeps dependencies isolated in node_modules.

```bash
# Python Project
my-python-project/
├── src/             # Source code root
│   └── my_project/  # Main package directory
├── tests/           # Test files
├── scripts/         # Utility scripts
└── docs/           # Documentation
```

Python's structure reflects its interpreted nature and package-based organization. There's no need for a compilation directory, and the main package usually mirrors the project name.

#### Python Project with Virtual Environment

```bash
my-python-project/
├── src/             # Source code root
│   └── my_project/  # Main package directory
├── tests/           # Test files
├── scripts/         # Utility scripts
├── docs/            # Documentation
└── venv/            # Virtual environment (not in version control)
```

> [!info] When a virtual environment is activated, this folder appears in your project, containing an isolated Python installation. More about this in the Environment Isolation section.

### Directory Roles and Organization

#### Source Code Organization

- **Node.js/TypeScript**:
    
    - `src/` contains source code that will be compiled
    - `dist/` holds the compiled JavaScript
    - Supports both feature-based and layered architectures
    - Compilation step transforms TypeScript into JavaScript
- **Python**:
    
    - Source code lives in a package directory
    - No compilation needed - Python is interpreted
    - Favors feature-based organization
    - Package name should match project name (with underscores)

The key difference here is that Python doesn't need a build step. Your source code is your running code, which simplifies the structure but requires more attention to organization.

#### Test Organization

- **Node.js/TypeScript**:

```
src/
  └── features/
      └── user.ts
      └── user.{test, spec}.ts  # Tests alongside code
# OR
tests/
  └── features/
      └── user.{test, spec}.ts  # Tests in separate directory
```

TypeScript projects often co-locate tests with source code, following the React/Jest convention. This makes it easier to maintain test and source code together.

- **Python**:

```
tests/
  └── test_users.py     # Always in separate directory
  └── test_models.py
```

Python strongly favors keeping tests in a separate directory. This is a convention followed by most Python testing tools and frameworks.

## 2. Naming Conventions

Understanding naming conventions is crucial as they differ significantly between ecosystems.

#### Files and Directories

- **Node.js/TypeScript**:

```
my-project/            # kebab-case for directories
  └── src/
      └── UserService.ts   # PascalCase for classes
      └── apiClient.ts     # camelCase for modules
```

TypeScript follows JavaScript conventions with mixed case styles depending on the type of code.

- **Python**:

```
my_project/            # snake_case for directories
  └── src/
      └── user_service.py  # snake_case for everything
      └── api_client.py
```

Python consistently uses snake_case for everything. This is a strong convention in the Python community and affects readability and tool compatibility.

## 3. Configuration Files

Configuration management differs significantly between Python and Node.js. Understanding these differences helps you maintain and configure Python projects effectively.

### Primary Configuration

#### Node.js/TypeScript

```json
// package.json - main configuration file
{
  "name": "my-project",
  "version": "1.0.0",
  "scripts": {
    "start": "node dist/index.js",
    "build": "tsc",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

Node.js centralizes most configuration in package.json, making it the one-stop shop for project configuration.

#### Python - Modern Approach

```toml
# pyproject.toml - modern approach
[tool.poetry]
name = "my-project"
version = "1.0.0"
description = ""

[tool.poetry.dependencies]
python = "^3.9"
flask = "^2.0.1"
```

The modern Python approach with pyproject.toml is more similar to package.json. This standard (defined in PEP 621) is becoming increasingly accepted and provides a more unified configuration experience across tools.

#### Python - Traditional Approach

```python
# setup.py - traditional approach
from setuptools import setup, find_packages

setup(
    name="my_project",
    version="1.0.0",
    packages=find_packages(),
    install_requires=[
        "flask>=2.0.1",
    ],
)
```

The traditional setup.py approach is still common in many projects. It's more flexible but requires Python code for configuration.

### Additional Configuration Files

Both ecosystems use multiple configuration files for different tools and purposes. Understanding their roles helps manage project configuration effectively.

```bash
# Node.js/TypeScript
my-node-project/
├── package.json      # Main configuration
├── tsconfig.json     # TypeScript configuration
├── jest.config.js    # Test configuration
├── .eslintrc        # Linting
└── .prettierrc      # Formatting
```

The Node.js/TypeScript ecosystem tends to have separate configuration files for each tool, but they often reference each other and share settings.

```bash
# Python
my-python-project/
├── pyproject.toml   # Main configuration (modern)
├── setup.cfg        # Tools configuration
├── pytest.ini      # Test configuration
├── .flake8         # Linting
└── .editorconfig   # Formatting
```

Python is moving towards consolidating configuration in pyproject.toml, but many tools still use their own configuration files. This is similar to the Node.js approach.

### `.editorconfig` for Consistent Styling

Both Python and Node.js projects can benefit from using `.editorconfig` files to maintain consistent coding styles across different editors and IDEs:

```ini
# .editorconfig
root = true

[*]
end_of_line = lf
insert_final_newline = true
charset = utf-8
trim_trailing_whitespace = true

[*.py]
indent_style = space
indent_size = 4

[*.{js,ts,json}]
indent_style = space
indent_size = 2
```

This configuration helps maintain consistent indentation and file formatting regardless of the editor or IDE used by team members.

## 4. Key Project Files

Understanding the role and importance of each project file is crucial for effective project management. While some files are similar between ecosystems, their purposes and contents often differ.

### Essential Files Comparison

```bash
my-node-project/
├── package.json        # [REQUIRED] Project definition
├── package-lock.json   # [REQUIRED] Dependency lock
├── tsconfig.json       # [REQUIRED] TS configuration
├── .gitignore         # [RECOMMENDED] Git ignore rules
└── README.md          # [RECOMMENDED] Documentation
```

The Node.js ecosystem relies heavily on package.json and automatically generates package-lock.json to ensure dependency consistency.

```bash
my-python-project/
├── pyproject.toml     # [REQUIRED] Project definition (modern)
├── setup.py           # [REQUIRED] Project definition (traditional)
├── requirements.txt   # [REQUIRED] Dependency list
├── .gitignore        # [RECOMMENDED] Git ignore rules
└── README.md         # [RECOMMENDED] Documentation
```

Python projects might have either pyproject.toml or setup.py (or both), depending on their tooling choices. This reflects Python's transition from traditional to modern tooling.

### Role of Key Files

Understanding the purpose of each file helps you maintain and configure projects correctly.

#### package.json vs pyproject.toml/setup.py

- **package.json**:
    
    - Central configuration hub for Node.js projects
    - Manages all dependencies in one place
    - Defines npm scripts for common tasks
    - Contains project metadata
    - Configures various development tools
- **pyproject.toml/setup.py**:
    
    - Defines project packaging and distribution
    - Handles build system requirements
    - Manages dependencies (in modern tooling)
    - Less focused on development scripts
    - More focused on distribution and packaging

The key difference is that Python separates concerns more strictly between development tooling and package distribution.

#### Development vs Production Files

- **Node.js/TypeScript**:
	- Clear separation of dependencies in package.json:

	```json
	{
	  "dependencies": {
	    "express": "^4.17.1"
	  },
	  "devDependencies": {
	    "typescript": "^4.5.4",
	    "jest": "^27.0.0"
	  }
	}
	```

- Source maps and TypeScript configuration handle development-production differences
    - Build process managed by TypeScript compiler
- **Python**:
    
    - Separate requirements files for different environments:

	```
	requirements/
	├── base.txt          # Shared dependencies
	├── development.txt   # Development tools
	└── production.txt    # Production-specific
	```

	- No compilation step between development and production
	- Environment differences handled by dependency management

## 5. Popular Python IDEs and Their Project Settings

Unlike the TypeScript ecosystem where VS Code dominates, Python developers use a variety of IDEs with different project configurations:

#### PyCharm

PyCharm creates a `.idea` directory with XML configuration files:

```bash
my-python-project/
├── .idea/            # PyCharm configuration
│   ├── misc.xml      # Python interpreter settings
│   └── modules.xml   # Project structure
```

PyCharm automatically detects virtual environments and offers powerful debugging and refactoring tools specifically designed for Python.

#### Visual Studio Code

VS Code uses a `.vscode` directory with JSON configurations:

```bash
my-python-project/
├── .vscode/
│   ├── settings.json      # Project-specific settings
│   └── launch.json        # Debugging configurations
```

When working with Python in VS Code, the Python extension creates settings for the interpreter path, linting, and formatting.

#### Jupyter Notebook Integration

Python projects often include Jupyter notebooks, which are not common in TypeScript:

```bash
my-python-project/
├── notebooks/
│   ├── analysis.ipynb
│   └── experiments.ipynb
```

These interactive notebooks combine code, visualizations, and documentation, and are especially popular in data science projects.

>[!info] Juipter Notebooks will be explained in one of the following lectures [[S3L4 Jupyter Notebooks|HERE]]

## 6. Quick Reference: Python vs Node.js Project Structure

|Aspect|Node.js/TypeScript|Python|
|---|---|---|
|Source code location|src/|package_name/ or src/package_name/|
|Built/compiled code|dist/|N/A (interpreted)|
|Tests|src/**/*.test.ts or tests/|tests/|
|Dependencies|node_modules/|venv/lib/python3.x/site-packages/|
|Package manager|npm, yarn|pip, poetry, pipenv|
|Main config file|package.json|pyproject.toml or setup.py|
|Naming style|camelCase, PascalCase|snake_case|
|Version control ignore|.gitignore|.gitignore|

## Tips for TypeScript Developers

When moving from TypeScript to Python, keep these key points in mind:

1. **Project Structure**:
    
    - Embrace Python's simpler structure - no need for separate source and distribution directories
    - Use snake_case consistently for all names
    - Keep tests in a dedicated directory
    - Remember that Python's import system is more strict than Node.js
2. **Configuration**:
    
    - Choose between modern (pyproject.toml) and traditional (setup.py) approaches
    - Don't mix different dependency management tools in the same project
    - Consider using Poetry for a more npm-like experience
    - Remember that Python tools are more independent than Node.js tools
3. **Project Files**:
    
    - Maintain clear dependency specifications
    - Use virtual environments
    - Keep documentation up to date
    - Follow Python community conventions for file organization

> [!info] Virtual environments will be covered in the next lecture

4. **Best Practices**:
    - Start new projects with modern tools (Poetry/pyproject.toml)
    - Create clear separation between production and development dependencies
    - Document project setup and requirements clearly
    - Use consistent naming conventions throughout the project

## Resources
- [What is the best project structure for a Python application? - Stack Overflow](https://stackoverflow.com/questions/193161/what-is-the-best-project-structure-for-a-python-application)
- [What is the optimal structure for a Python project? : r/Python](https://www.reddit.com/r/Python/comments/18qkivr/what_is_the_optimal_structure_for_a_python_project/)

## Backlinks
```dataview
TABLE WITHOUT ID 
file.inlinks as Links,
file.mtime as Modified
WHERE file.name = this.file.name
```