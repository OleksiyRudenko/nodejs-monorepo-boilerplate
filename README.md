# NodeJS Monorepo Boilerplate

Requirements:
- sub-projects have their own dependencies (and a `package.json`)
- repo root sets general rules (e.g. linting)
- adding a sub-project must be as hassle-free as only possible
- CI/CD builds must not fail

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Table of Contents

- [Installation](#installation)
  - [What's inside](#whats-inside)
  - [Known issues](#known-issues)
    - [Linter throws error when there are no `.js` files](#linter-throws-error-when-there-are-no-js-files)
    - [`npm-run-all` causes error when there are no scripts matching the pattern](#npm-run-all-causes-error-when-there-are-no-scripts-matching-the-pattern)
- [Adding a sub-project](#adding-a-sub-project)
- [Sub-projects Development](#sub-projects-development)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- generated with [DocToc](https://github.com/thlorenz/doctoc) -->

## Installation

Run `yarn` to install dev dependencies.

### What's inside

* ES Linting (see [`./package.json`](./package.json))
* Travis CI config (see [`./.travis.yml`](./.travis.yml))
* Nested projects installation with
  [`npm-run-all`](https://www.npmjs.com/package/npm-run-all)
  (see [`./package.json`](./package.json))
* [`./.gitignore`](./.gitignore) makes root and any nested
  `node_modules` ignored

### Known issues

#### Linter throws error when there are no `.js` files

```
ESLint: 6.6.0.

No files matching the pattern "." were found.
Please check for typing mistakes in the pattern.
```

There is a planned [fix for this](https://github.com/eslint/eslint/pull/12377)

**Workaround employed:**
just added empty `./index.js` to suppress
the error.

Once the changes introduced with the PR above
become a part of eslint release the following AP are to be taken:
* delete `./index.js`
* in `./package.json` replace `"lint:js": "eslint .",`
  with `"lint:js": "eslint --no-error-on-unmatched-pattern .",`

#### `npm-run-all` causes error when there are no scripts matching the pattern

When there are no scripts matching the pattern `install:**`
an error is thrown.

```
$ run-p install:**
ERROR: Task not found: "install:**"
error Command failed with exit code 1.
```
**Workaround employed:**
a 'void' script (`"install:void": ":"`) is used to suppress
the error. `:` command is effectively an explicit 
no-op shell command.

If `npm-run-all` owner decides
to [add a possibility](https://github.com/mysticatea/npm-run-all/issues/182) 
to suppress the error the following AP are to be taken:
* in `./package.json` delete `"install:void": ":",`

## Adding a sub-project

Add a project in a sub-folder with its own `package.json`.

In repo root `package.json` add a script following the pattern:

`"install:<sub-project-name>": "cd ./path/to/sub-project && yarn"`

It is recommended to add the new script somewhere above `preinstall`
script so that a commit into `./package.json` affects only
a single row in the file.

Check the `sub-project` branch as a reference.

`git checkout sub-project; git diff HEAD^ ./package.json`
will reveal the change in the root `package.json` introduced
by the sub-project.

## Sub-projects Development

Develop normally. Run `yarn run lint:js` before pushing changes.
