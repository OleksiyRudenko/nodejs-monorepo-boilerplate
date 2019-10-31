# NodeJS Monorepo Boilerplate

Requirements:
- sub-projects have their own dependencies (and `package.json`)
- repo root sets general rules (e.g. linting)
- adding a sub-project must be as hassle-free as only possible
- CI/CD builds must not fail

## Installation

Run `yarn` to install dependencies.

## Adding a sub-project

Add a project in a sub-folder with its own `package.json`.

In repo root `package.json` add...

## Development

Run `yarn run lint:js` before pushing changes.
