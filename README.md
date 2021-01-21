# Egghead

## Create a new empty Nx workspace

```bash
npx create-nx-workspace <workspace name>
```

Choose

  Empty workspace
  Nx Cloud
  Nx CLI

### Useful Commands

```bash
yarn nx list
```

Shows all the available packages for nx

```bash
yarn nx list @nrwl/react
```

## Generate a new React app with Nx

``bash
yarn add @nrwl/react
yarn nx g @nrwl/react:application --name store
``

### Useful Commands


``bash
yarn nx g @nrwl/react:application --help
``

## Running the application

```bash
yarn nx run store:serve
```

You can modify the port in `workspace.json`:
```diff
  "serve": {
    "executor": "@nrwl/web:dev-server",
    "options": {
      "buildTarget": "store:build",
+     "port": 3000
  },
```

## Generate a shared react lib for store application

```bash
yarn nx g @nrwl/react:lib ui-shared --directory=store
```

### Generate a component inside lib

```bash
yarn nx g @nrwl/react:component header --project=store-ui-shared
```

Choose `Y` to export the component.

### Using the generated component inside application

You can find the component import path from `tsconfig.base.json`:

```json
    "paths": {
      "@egghead/store/ui-shared": ["libs/store/ui-shared/src/index.ts"]
    }
```

app.tsx:

```typescript
import {Header} from '@egghead/store/ui-shared'
```

## Genearte a Typescript lib

```bash
yarn nx g @nrwl/workspace:lib util-formatters --directory=store
```

## Generate a Lib for application by --appProject

```bash
 yarn nx g @nrwl/react:lib feature-game-detail --directory=store --appProject=store
 ```

 This will add some routing config into application

 ## Add a backend server
 
 ```bash
 yarn add -D @nrwl/express
 ```

```bash
yarn nx g @nrwl/express:application api --frontendProject=store
```

Added `--frontendProject` will also generate a `proxy.conf.json` file in `store` application.

```bash
yarn nx run api:serve
```

## Useful commands

### Run Frontend and backend in one command

```bash
yarn nx run-many --target=serve --projects=api,store --parallel=true
```

### Modify `workspace.json` to run multi applications

```json
        "serve": {...},
        "serveAppAndApi": {
          "builder": "@nrwl/workspace:run-commands",
          "options": {
            "commands": [
              {
                "command": "nx run api:serve"
              },
              {
                "command": "nx run store:serve"
              }
            ]
          }
        },
```

Run:

```bash
yarn nx run store:serveAppAndApi
```


## Generate a lib which share between backend and frontend

```bash
yarn nx g @nrwl/workspace:lib util-interface --directory=api
```

It will generate under `libs/api/util-interface`.

## Use storybook 

```bash
yarn add @nrwl/storybook -D
yarn nx list @nrwl/react
```

You should be able to see `storybook-configuration`.

```bash
yarn nx generate @nrwl/react:stroybook-configruation store-ui-shared --configureCypress --generateStories
```

It will generate `Storybook` under `libs/ui-shared/.storybook` & `Cypress` under `store-ui-shared-e2e` folder

### Run storybook

```bash
yarn nx run store-ui-shared:storybook
```

### Run Cypress

```bash
yarn nx run store-ui-shared-e2e:e2e --watch
```


### Run JEST

```bash
yarn nx run store:test
```

## Build application

```bash
yarn nx run store:build --configuration=production
```

or 

```bash
yarn nx build store --configuration=production
```

Will generate a `dist` folder

## Lint application

```bash
yarn nx run store:lint
```

## Run the applications/libs which affected

`Nx` use `git` history to detect which applications or libs have been changed.

And then run the affected libs and applications to speed up testing.

```bash
yarn nx affected:test --base=master
yarn nx affected:lint --base=master
yarn nx affected:dep-graph --base=master
```

Run unit testings based on master branch.

```bash
yarn nx affected:test --all
yarn nx affected:test --all --skip-nx-cache
```

`Nx` will cache the running `affected` result into `node_modules/.cache` to speed up next runtime.

You can `--skip-nx-cache` or delete cache.

## Migrations

```bash
yarn nx migrate latest
```

then install the new packages.

```bash
yarn
```

If there is `migrations.json` created:

```bash
yarn nx migrate --run-migrations=migrations.json
```


