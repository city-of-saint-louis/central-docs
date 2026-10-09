# Golden Rules

These rules apply to all work. Any exception must be explained and justified in the PR description.

---

## Security Hygiene
Never commit credentials, keys, or `.env` files. If one is committed by accident, report it to a senior developer or GitHub admin right away. Deleting it in a later commit isn't enough because it stays in the history. Never commit real personal or resident data, including in tests, sample files, or screenshots.

## Ask Before Big Decisions
Raise significant design decisions or scope changes with a senior developer before building, not during review.

## Branching
Never push directly to `main`. All work happens through a branch and PR.

## Focus Your Commits
Each commit must do one thing, and the project must still build and work at that commit.

## Linting and Formatting
Use the linting and formatting tools configured in the repository. Don't change their configuration or introduce different tools without approval, and don't reformat code unrelated to your change. If a repo has no tooling configured, ask before adding any.

## Tests
New work must include tests. Refactored code that didn't already have coverage must include tests. CI must pass before merging. Backfilling coverage gaps should be handled in a dedicated PR.

## Documentation
Update the README and other relevant docs when setup (e.g. new environment variables, build steps) or behavior changes.

## PR Scope
Keep your PR focused around a single purpose. When opening a PR, ask the question "Could this be reviewed in one sitting by one person?" If the answer is no, split it up. Commit messages and PR descriptions must explain what changed and why. Link the related ticket or issue if one exists.

## Dependency Changes
Changes that add, remove, or update a package must be made in a dedicated PR. If a feature you're working on requires a dependency change, the dependency PR must precede your feature.

## Keep Main Deployable
Merged work must not break anything or expose unfinished features to users. If a feature is too large for one PR, split it into pieces that each work on their own, or keep unfinished parts unreachable until the final PR connects them.

## Merging
Once your PR is approved by a reviewer, merge it yourself and perform any post-merge testing needed. Don't merge without an approval, even if the repository allows it. Senior developers and GitHub admins may merge without approval when necessary.