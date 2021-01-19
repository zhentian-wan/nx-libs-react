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
