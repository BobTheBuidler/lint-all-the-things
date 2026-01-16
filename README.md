# Lint All The Things

Composite GitHub Action to run [pyupgrade](https://github.com/asottile/pyupgrade), [isort](https://github.com/PyCQA/isort), [black](https://github.com/psf/black), and [autoflake](https://github.com/PyCQA/autoflake) on your Python codebase.  
Automatically commits once (via [Poosh](https://github.com/BobTheBuidler/poosh)), pushes, or opens a PR if any of the tools modify your code—using a commit message that lists which linters actually changed files, with the same detail as the individual commits (e.g., pyupgrade target version, `black .`, autoflake flags).

## Features

- Runs pyupgrade, isort, black, and autoflake in sequence (respecting any repo config files)
- Auto-commits once with Poosh, and the commit message lists only the linters that changed files (keeping detailed commands/versions)
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
2. Installs and runs pyupgrade, isort, black, and autoflake (in that order; each tool uses your repo config if present)
3. After each tool, checks for changes and builds a list of linters that modified code
4. Commits once (via Poosh) with a message that lists the linters that made changes—preserving details like pyupgrade target, `black .`, and autoflake flags—then pushes or opens a PR

**Note:**  
- This action expects you to have already checked out your code (use actions/checkout before this action).
- Commits target the current branch (`github.head_ref` or `github.ref_name`). If a direct push fails (protected branch or fork PR), Poosh opens a PR instead; for fork PRs it targets the base branch.
- If no changes are made by any tool, no commit is created.

## Example Commit Messages

- `chore: lint (pyupgrade --py311-plus; isort; black .)`
- `chore: lint (black .)`

## Testing

- CI runs an inline Node harness (see `.github/workflows/tests.yml`) that stubs linters/Poosh, uses temp repos, and never commits to this repository. You can copy the inline script from the workflow step to run locally if desired.

## License

MIT
