# Bootstrapping

Bootstrapping your monorepo is very similar to how you would setup a regular JavaScript project. The only contraint this guide enforces is that you use [*yarn*](https://yarnpkg.com/lang/en/) instead of *npm* to manage packages. The reason for this constraint are describe throughout this guide. For now, you only need to know that *yarn* has some additional features managing monorepos that *npm* lacks.

**Prerequisite:** Make sure you have installed a current version of [*Node.js*](https://nodejs.org) (v10+) and [*yarn*](https://yarnpkg.com/lang/en/) at least in version *1.0*. A newer version of *yarn*  is of course even better.

#### 1. Create a project folder

Nothing special here. Run `mkdir <my project name>` inside a folder where you want to have the new repository.

#### 2. Create a `package.json` file**

After you switched to the folder you just created run `yarn init --yes`. This will create a `package.jons` similar to this one:

```json
{
  "name": "monorepo-guide",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}
```
You can also remove the `main` property if you want, since it will not be used. If you have already initialized a git repository, *yarn* will also automatically add a `repository` field.

#### 3. Adding workspaces

To add support for hosting multiple modules, *yarn* has a feature called "[workspaces](https://yarnpkg.com/lang/en/docs/workspaces/)". In order to enable this feature, you have to add the following properties to your `package.json`:

```diff
{
  "name": "monorepo-guide",
  "version": "1.0.0",
  "license": "MIT",
+ "private": true,
+ "workspaces": [
+   "packages/*"
+ ]
}
```

Flagging your project as `private` is required and is a precaution to not accidentally publish your whole repo to the registry. The `workspaces` property is used to define the location of your modules. In the above example all subfolders of `workspaces` are treated as like a module by *yarn*. This also means that every subfolder needs its own `package.json`. So remember, whenever you add a new module, there should at least be a minimal `paackge.json` inside it.

```
<root>
├── package.json
├── (...)
└── packages/
    ├── module-1/
    │   └── package.json
    ├── (...)
    └── module-n/
        └── package.json
```

You may have noticed that the `workspaces` property is an array. If your project grows or if you want to further categorize your modules, you can change the array accordingly.

For example, if your app follows DDD-esque style you may want to have a folder that contains alls your domains and another one that has all server related modules inside. Maybe even an extra folder for UI-related modules?

Your project may look something like this:

**package.json**

```json
{
  "name": "monorepo-guide",
  "version": "1.0.0",
  "license": "MIT",
  "private": true,
  "workspaces": [
    "domain/*",
    "server/*",
    "ui/*",
  ]
}
```

- add folder struture
- not necessary (for example if you create a omponent library as monorepo)

---

- setup root folder
- regular (node) project setup
- except we are using `yarn` instead of `npm`
- why?
- it's 2019 and `yarn` objectively isn't better anymore than `npm`
- BUT: `yarn` supports workspaces
- workspaces have some neat features that help manage a monorepo
- like symlinking (all modules are inside a root `node_modules` folder)