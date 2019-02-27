# Bootstrapping

Bootstrapping your monorepo is very similar to how you would setup a regular JavaScript project. The only contraint this guide enforces is that you use [*yarn*](https://yarnpkg.com/lang/en/) instead of *npm* to manage packages. The reason for this constraint are describe throughout this guide. For now, you only need to know that *yarn* has some additional features managing monorepos that *npm* lacks.

**Prerequisite:** Make sure you have installed a current version of [*Node.js*](https://nodejs.org) (v10+) and [*yarn*](https://yarnpkg.com/lang/en/) at least in version *1.2.0*.

**1. Create a project folder**

Nothing special here. Run `mkdir <my project name>` inside a folder where you want to have the new repository.

**2. Create a `package.json` file**

After you switched to the folder you just created run `yarn init --yes`. This will create a `package.jons` similar to this one:

```json
{
  "name": "monorepo-guide",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}
```

In order to make sure you don't accidentally publish the whole monorepo to *npm* I recommend to flag the project as private by adding a property `private` with value `true` to the JSON. You can also remove the `main` property, since it will not be used.

```json
{
  "name": "monorepo-guide",
  "version": "1.0.0",
  "private": true
}
```

---

- setup root folder
- regular (node) project setup
- except we are using `yarn` instead of `npm`
- why?
- it's 2019 and `yarn` objectively isn't better anymore than `npm`
- BUT: `yarn` supports workspaces
- workspaces have some neat features that help manage a monorepo
- like symlinking (all modules are inside a root `node_modules` folder)