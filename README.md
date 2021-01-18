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