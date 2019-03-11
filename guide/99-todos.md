TODO: Need a place to put these sections

---

## Installing local dependencies

When adding dependencies to your modules _yarn_ will always try to find the depdendency in the registry when no version is speficied. This is fine most of the time, but [can be cumbersome if you want to add a local dependency](https://github.com/yarnpkg/yarn/issues/4878). Using _lerna_

Recommended alternative: `yarn add <package>@*` (because you always WANT the latest version of your local dependency).

- Note about: https://github.com/yarnpkg/yarn/issues/4878#issuecomment-386635234

---

## Testing with `jest`

- Run TS not JS files
- Use `tsconfig.json` in root and path mapping
- Transpilation on the fly

```json
{
  "extends": "@monorepo/tsconfig",
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@monorepo/*": ["packages/*/src"]
    }
  }
}
```

---

## Share configuration as packages

- e.g. tsconfig, eslint, prettier
