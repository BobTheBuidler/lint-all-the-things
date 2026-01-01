# Lint All The Things

Composite GitHub Action to run [pyupgrade](https://github.com/asottile/pyupgrade), [black](https://github.com/psf/black), and [autoflake](https://github.com/PyCQA/autoflake) on your Python codebase.  
Automatically commits and pushes changes if any of the tools modify your code.

## Features

- Runs pyupgrade, black, and autoflake in sequence
- Auto-commits and pushes changes for each tool if needed
- Configurable Python version (default: 3.12)
- Designed for use in PR workflows

## Usage

Add this to your workflow (after checking out your code):

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
        with:
          ref: ${{ github.head_ref }}
      - uses: BobTheBuidler/lint-all-the-things@master
        with:
          python-version: 3.12
```

## Inputs

- **python-version** (required): Python version to use

## How it works

1. Sets up Python
2. Installs and runs pyupgrade, black, and autoflake (in that order)
3. After each tool, checks for changes and auto-commits/pushes if needed

**Note:**  
- This action expects you to have already checked out your code (use actions/checkout before this action).
- Each tool's changes are committed separately, with clear commit messages.
- If no changes are made by a tool, no commit is created for that tool.

## Example Commit Messages

- `chore: run \`pyupgrade\` for Python3.10`
- `chore: \`black .\``
- `chore: autoflake`

## License

MIT
