name: code-morpher
description: Transforms legacy codebases into modern, maintainable structures by refactoring outdated patterns, upgrading dependencies, and enforcing contemporary coding standards.
tags: [code, refactoring, modernization, architecture]
author: Kilo
version: 1.0.0
dependencies:
  - Node.js (v16+ for JavaScript/TypeScript projects)
  - Python (v3.8+ for Python projects)
  - Git (v2.30+ for version control)
  - ESLint/Prettier for JS/TS linting
  - Black/Flake8 for Python formatting
  - Docker (optional, for containerized builds)
environment_variables:
  - CODE_MORPHER_TARGET_DIR: Absolute path to the codebase directory (default: current working directory)
  - CODE_MORPHER_BACKUP_ENABLED: Set to 'true' to create automatic backups before transformations (default: true)
  - CODE_MORPHER_LINT_LEVEL: Set to 'strict', 'moderate', or 'lenient' for linting enforcement (default: moderate)
last_updated: 2026-03-13
---

# Code Morpher Skill

## Purpose

Code Morpher is designed to modernize and refactor legacy codebases, addressing common issues like outdated syntax, inefficient patterns, deprecated dependencies, and poor maintainability. It transforms code into contemporary standards while preserving functionality and improving performance.

### Real Use Cases

- **Legacy JavaScript Migration**: Convert ES5 code to ES6+ modules, replace var with let/const, and implement arrow functions in a 10-year-old Node.js application.
- **Python 2 to 3 Upgrade**: Automate the conversion of Python 2.x scripts to Python 3.x, handling print statements, unicode handling, and import changes in a data processing pipeline.
- **React Class to Functional Components**: Refactor class-based React components to functional hooks, updating lifecycle methods to useEffect and state management to useState in a large-scale web app.
- **Database Query Optimization**: Modernize raw SQL queries to use ORM patterns (e.g., Sequelize for Node.js or SQLAlchemy for Python), adding proper error handling and connection pooling.
- **API Endpoint Refactoring**: Convert RESTful endpoints to GraphQL resolvers or update to OpenAPI 3.0 standards, including authentication middleware integration.

## Scope

Code Morpher operates on the following specific commands and transformations:

- **transform-js**: Refactors JavaScript/TypeScript files (flags: --target=es6, --target=es2022, --preserve-comments=true)
- **transform-py**: Modernizes Python code (flags: --from-py2, --from-py3.6, --add-type-hints)
- **refactor-components**: Converts UI components to modern patterns (flags: --framework=react, --framework=vue, --add-tests=true)
- **upgrade-deps**: Updates package.json or requirements.txt dependencies (flags: --major-only=false, --check-vulnerabilities=true)
- **optimize-queries**: Refactors database interactions (flags: --orm=sequelize, --orm=sqlalchemy, --add-migrations=true)
- **lint-and-fix**: Applies automatic linting and formatting fixes (flags: --tool=eslint, --tool=black, --fix-only=true)

## Detailed Work Process

1. **Initialization**: Analyze the target directory for supported languages and frameworks. Create a backup snapshot using `git tag backup-pre-morph-<timestamp>`.
2. **Dependency Assessment**: Scan package.json/requirements.txt for outdated versions. Flag vulnerabilities using `npm audit` or `pip-audit`.
3. **Pattern Detection**: Use AST parsing to identify legacy patterns (e.g., var declarations, callback hell, synchronous I/O).
4. **Transformation Execution**: Apply targeted refactors in batches, running tests after each major change to ensure stability.
5. **Linting and Formatting**: Execute ESLint/Prettier or Black/Flake8 to enforce style consistency.
6. **Testing Integration**: Generate or update unit tests for refactored code, ensuring 80%+ coverage.
7. **Final Verification**: Run full test suite and lint checks. Generate a transformation report detailing changes made.

## Golden Rules

- Always create backups before transformations; never modify production code without staging.
- Preserve existing functionality; transformations must pass all existing tests.
- Handle imports and dependencies carefully; broken imports can cascade errors.
- Use incremental changes; avoid massive rewrites that introduce multiple bugs.
- Document all changes in commit messages with clear before/after examples.
- Respect project conventions; mimic existing code style and patterns.
- Test thoroughly; run the full test suite after each transformation batch.

## Examples

### Example 1: JavaScript ES5 to ES6 Migration
**User Prompt**: `openclaw code-morpher transform-js --target=es2020 --preserve-comments=true /path/to/legacy-app/src`

**Input Code** (legacy.js):
```javascript
var express = require('express');
var app = express();

function handleRequest(req, res) {
    res.send('Hello World');
}

app.get('/', handleRequest);
module.exports = app;
```

**Output Code** (modernized.js):
```javascript
import express from 'express';
const app = express();

const handleRequest = (req, res) => {
    res.send('Hello World');
};

app.get('/', handleRequest);
export default app;
```

**CLI Output**: Transformed 15 files, upgraded 8 dependencies, fixed 23 linting issues. Run `npm test` to verify.

### Example 2: Python 2 to 3 Upgrade
**User Prompt**: `openclaw code-morpher transform-py --from-py2 --add-type-hints /path/to/data-pipeline`

**Input Code** (legacy.py):
```python
import urllib2

def fetch_data(url):
    response = urllib2.urlopen(url)
    data = response.read()
    print data
    return data
```

**Output Code** (modernized.py):
```python
from typing import str
import urllib.request

def fetch_data(url: str) -> str:
    with urllib.request.urlopen(url) as response:
        data = response.read().decode('utf-8')
        print(data)
        return data
```

**CLI Output**: Migrated 42 files from Python 2 to 3, added type hints to 28 functions, updated imports for urllib.

### Example 3: React Class to Hooks Refactor
**User Prompt**: `openclaw code-morpher refactor-components --framework=react --add-tests=true /path/to/react-app/components`

**Input Code** (LegacyComponent.js):
```javascript
import React from 'react';

class LegacyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = { count: 0 };
    }

    componentDidMount() {
        console.log('Mounted');
    }

    increment = () => {
        this.setState({ count: this.state.count + 1 });
    }

    render() {
        return <button onClick={this.increment}>{this.state.count}</button>;
    }
}

export default LegacyComponent;
```

**Output Code** (ModernComponent.js):
```javascript
import React, { useState, useEffect } from 'react';

const ModernComponent = () => {
    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log('Mounted');
    }, []);

    const increment = () => {
        setCount(prevCount => prevCount + 1);
    };

    return <button onClick={increment}>{count}</button>;
};

export default ModernComponent;
```

**CLI Output**: Refactored 12 components to hooks, generated 8 new test files, updated 5 prop interfaces.

## Verification Steps

1. **Run Tests**: Execute `npm test` or `pytest` to ensure no regressions.
2. **Lint Check**: Run `npx eslint .` or `black --check .` to verify code style compliance.
3. **Build Verification**: Attempt a full build with `npm run build` or `python setup.py build` to catch compilation errors.
4. **Dependency Audit**: Use `npm audit` or `pip list --outdated` to confirm no new vulnerabilities.
5. **Performance Check**: Profile key functions for performance regressions using tools like Lighthouse or cProfile.

## Troubleshooting Common Issues

- **Syntax Errors After Transform**: Check for incomplete AST parsing; manually review the transformed file and adjust edge cases.
- **Import Failures**: Ensure all dependencies are updated in package.json/requirements.txt; run `npm install` or `pip install -r requirements.txt`.
- **Test Failures**: Transformations may change function signatures; update tests to match new APIs or revert specific changes.
- **Memory Issues**: For large codebases, process in smaller batches using `--batch-size=10` flag.
- **Framework-Specific Errors**: Verify framework compatibility (e.g., React version must support hooks); update framework if needed.
- **Rollback Needed**: If issues persist, use git to revert changes: `git reset --hard backup-pre-morph-<timestamp>`.

## Rollback Commands

- **Full Revert**: `git reset --hard backup-pre-morph-<timestamp>` (restores entire codebase to pre-transformation state).
- **Selective Undo**: `git revert <commit-hash>` (undoes specific transformation commits while preserving others).
- **Dependency Rollback**: `npm install <package>@<previous-version>` or `pip install <package>==<previous-version>` (reverts specific package updates).
- **File-Specific Restore**: `git checkout HEAD~1 -- <file-path>` (restores individual files from before the last commit).
- **Clean Revert**: `git clean -fd && git reset --hard HEAD` (removes untracked files and resets to last commit, useful if transformations added temporary files).