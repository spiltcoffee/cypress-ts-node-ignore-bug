# cypress-ts-node-ignore-bug

Reproduction of a bug in Cypress 12.10.0 introduced by https://github.com/cypress-io/cypress/pull/26305

## Expected behaviour

Executing `cypress run` with `NODE_OPTIONS=--preserve-symlinks` and a reference in the
`cypress.config.ts` to a typescript file in `node_modules` that is ignored by `ts-node.ignore` in
the `tsconfig.json` works.

(Look, I know it's convoluted, but I've got this situation currently...)

## Actual behaviour

Crashes with the following error:
```
Your configFile is invalid: <snip>\cypress.config.ts

It threw an error when required, check the stack trace below:

TypeError [ERR_UNKNOWN_FILE_EXTENSION]: Unknown file extension ".ts" for <snip>\cypress.config.ts
    at new NodeError (node:internal/errors:399:5)
    at Object.getFileProtocolModuleFormat [as file:] (node:internal/modules/esm/get_format:79:11)
    at defaultGetFormat (node:internal/modules/esm/get_format:121:38)
    at defaultLoad (node:internal/modules/esm/load:81:20)
    at nextLoad (node:internal/modules/esm/loader:163:28)
    at ESMLoader.load (node:internal/modules/esm/loader:605:26)
    at ESMLoader.moduleProvider (node:internal/modules/esm/loader:457:22)
    at new ModuleJob (node:internal/modules/esm/module_job:64:26)
    at ESMLoader.#createModuleJob (node:internal/modules/esm/loader:480:17)
    at ESMLoader.getModuleJob (node:internal/modules/esm/loader:434:34)
    at processTicksAndRejections (node:internal/process/task_queues:95:5)
```

## How to run reproduction

Clone the repo, then run the following:
```
yarn

# should break using 12.10.0
yarn test-a

# should work using 12.9.0
yarn test-b
```