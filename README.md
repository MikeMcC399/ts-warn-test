# "Couldn't find tsconfig.json" warning when no TypeScript in project

Issue logged to https://github.com/cypress-io/cypress/issues/25335

## Current behavior

If the npm module [browser-sync](https://www.npmjs.com/package/browser-sync) is installed, a warning message is displayed when Cypress is run:

"Couldn't find tsconfig.json. tsconfig-paths will be skipped"

There are however no TypeScript files involved in the [test](https://github.com/MikeMcC399/ts-warn-test/tree/main/tests/cy-with-browser-sync).

Installing [browser-sync](https://www.npmjs.com/package/browser-sync) 2.27.11 has caused [typescript](https://www.npmjs.com/package/typescript) 4.9.4 to be installed.

```text
ts-warn-test@1.0.0
└─┬ browser-sync@2.27.11
  └─┬ browser-sync-client@2.27.11
    └── typescript@4.9.4
```
### On GitHub ubuntu-22.04 runner

Without DEBUG enabled, the log file shows:

```text
Opening Cypress...
...
Couldn't find tsconfig.json. tsconfig-paths will be skipped
```

With DEBUG enabled, the log file shows:

```text
cypress:lifecycle:ProjectConfigIpc found typescript in /home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync
cypress:lifecycle:ProjectConfigIpc using cjs with --require /home/runner/.cache/Cypress/12.2.0/Cypress/resources/app/node_modules/@packages/server/lib/plugins/child/register_ts_node.js
cypress:lifecycle:ProjectConfigIpc trigger the load of the file
cypress:server:register-ts-node executing register_ts_node with args { _: [ '/opt/hostedtoolcache/node/18.12.1/x64/bin/node', '/home/runner/.cache/Cypress/12.2.0/Cypress/resources/app/node_modules/@packages/server/lib/plugins/child/require_async_child.js' ], projectRoot: '/home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync', file: '/home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync/cypress.config.js' }
cypress:server:register-ts-node registering ts-node for projectRoot: /home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync and file: /home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync/cypress.config.js
cypress:server:ts-node projectRoot path: /home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync
cypress:server:ts-node registeredFile: /home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync/cypress.config.js
cypress:server:plugins resolving typescript with projectRoot '/home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync'
cypress:server:plugins resolved typescript /home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync/node_modules/typescript/lib/typescript.js
cypress:server:ts-node typescript path: /home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync/node_modules/typescript/lib/typescript.js
cypress:server:ts-node registering project TS with options { compiler: '/home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync/node_modules/typescript/lib/typescript.js', compilerOptions: { module: 'commonjs', preserveValueImports: false }, dir: '/home/runner/work/ts-warn-test/ts-warn-test/tests/cy-with-browser-sync', transpileOnly: true }
Couldn't find tsconfig.json. tsconfig-paths will be skipped
```
### Run locally Ubuntu 22.04

On Ubuntu 22.04 run locally

```bash
cd tests/cy-with-browser-sync
npx cypress run
```
**no** warning is output, unless
```bash
export DEBUG=cypress:*
```
is set, in which case the warning
"Couldn't find tsconfig.json. tsconfig-paths will be skipped"
is found in the debug logs.

### Run locally Windows 11

On Windows 11 run locally

```bash
cd tests/cy-with-browser-sync
npx cypress run
```
the warning
"Couldn't find tsconfig.json. tsconfig-paths will be skipped"
is output.

## Desired behavior

Installing [browser-sync](https://www.npmjs.com/package/browser-sync) without any TypeScript files (*.ts, *.tsx) should not cause a warning message to appear when Cypress is run.

## Test code to reproduce

See repository https://github.com/MikeMcC399/ts-warn-test

Workflows:

- https://github.com/MikeMcC399/ts-warn-test/blob/main/.github/workflows/cypress-test.yml (no DEBUG)
- https://github.com/MikeMcC399/ts-warn-test/blob/main/.github/workflows/cypress-test-debug.yml (with `DEBUG:cypress:*`)

Or clone the repository and run

```bash
cd tests/cy-with-browser-sync
npx cypress run
```

## Cypress version

- 12.2.0
- 12.5.1
- 12.6.0
- 12.8.1

## Node version

- 16.13.0 for GitHub runner using [cypress-io/github-action](https://github.com/cypress-io/github-action)
- 16.16.0

## Operating System

ubuntu-22.04

## Debug logs

- Action log (https://github.com/MikeMcC399/ts-warn-test/actions/runs/3828987111/jobs/6515177387)
- https://github.com/MikeMcC399/ts-warn-test/blob/main/logs/3_Cypress%20run.txt

## Other

- This issue is related to the open issue https://github.com/cypress-io/cypress/issues/22273.
- and to https://github.com/cypress-io/cypress/issues/21805

It is not the same issue though, because in this issue a project is described which is not using TypeScript at all.
