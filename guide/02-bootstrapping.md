# Bootstrapping

Bootstrapping your monorepo is very similar to how you would setup a regular JavaScript project. The only contraint this guide enforces is that you use [_yarn_](https://yarnpkg.com/lang/en/) instead of _npm_ to manage packages. The reason for this constraint are describe throughout this guide. For now, you only need to know that _yarn_ has some additional features managing monorepos that _npm_ lacks.

**Prerequisite:** Make sure you have installed a current version of [_Node.js_](https://nodejs.org) (v10+) and [_yarn_](https://yarnpkg.com/lang/en/) at least in version _1.0_. A newer version of _yarn_ is of course even better.

## Setup a monorepo with `yarn`

### 1. Create a project folder

Nothing special here. Run `mkdir <my project name>` inside a folder where you want to have the new repository.

### 2. Create a `package.json` file\*\*

After you switched to the folder you just created run `yarn init --yes`. This will create a `package.jons` similar to this one:

```json
{
  "name": "monorepo-guide",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}
```

You can also remove the `main` property if you want, since it will not be used. If you have already initialized a git repository, _yarn_ will also automatically add a `repository` field.

### <a name="adding_workspaces"></a>3. Adding workspaces

To add support for hosting multiple modules, _yarn_ has a feature called "[workspaces](https://yarnpkg.com/lang/en/docs/workspaces/)". In order to enable this feature, you have to add the following properties to your `package.json`:

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

Flagging your project as `private` is required and is a precaution to not accidentally publish your whole repo to the registry. The `workspaces` property is used to define the location of your modules. In the above example all subfolders of `workspaces` are treated like a module by _yarn_. This also means that every subfolder needs its own `package.json`. So remember, whenever you add a new module, there should at least be a `paackge.json` inside it.

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

For example, if your app follows DDD-like style you may want to have a folder that contains all your domains and another folder that has all server related modules inside. Maybe even an extra folder for UI-related modules?

Your workspaces setup may look something like this:

**package.json**

```json
{
  "name": "monorepo-guide",
  "version": "1.0.0",
  "license": "MIT",
  "private": true,
  "workspaces": ["domain/*", "server/*", "ui/*"]
}
```

## Benefits of using yarn

**But why would you use and need workspaces?** Until this point there is no distinction between the setup described here and just adding these folder and declare them as "this is where our modules will live". In fact, a lot of people do this and may end up with a not so beautiful monolith. Using _yarn_ and workspaces has no benefit yet.

As already mentioned, every module has to have a `package.json` in its root folder. This contraint helps _yarn_ to auotmatically symlink all _node modules_ in your monorepo. The modules themselves will not contain a `node_modules` folder anymore.[¹](#footnote_1) Instead all dependencies are hoisted in the root `node_modules` folder, which will lead to a much smaller `_node_modules` folder since there are less duplicates.

Also, you can run `yarn install` in the root of your repository and all dependencies (indluding the ones of your modules) will be installed for you. No need to switch between folders and run the install command in each and everyone of them.

Executing scripts in the monorepo is also easier with _yarn_. Using [`workspace`](https://yarnpkg.com/lang/en/docs/cli/workspace/) or [`workspaces`](https://yarnpkg.com/lang/en/docs/cli/workspaces/) respectively allows you to execute commands in the scope of a module.

## Adding `lerna` for DX

> Lerna is a tool that optimizes the workflow around managing multi-package repositories with git and npm.

While this statement sound very similar to what _yarn_ already does, _lerna_ comes with some much appreciated appadditional features, like automatic changelog creation or executing an arbitrary command (like `rm -rf ./node_modules`) in each package.

_lerna_ is also a little bit more developer friendly than _yarn_ when it comes to executing scripts. For example, executing `$ yarn workspaces run build` will try to run the `build` [npm script](https://docs.npmjs.com/misc/scripts). But if a modules does not have that script _yarn_ will exit with a non-zero code. This can be very annoying. Especially if you try to run a script inside a CI. With _lerna_ you can run `$ lerna run <script name>` and if a module does not have that npm script, _lerna_ will just skip that module.

### Install `lerna`

To install lerna run the following command inside your repository root:

```
$ npx lerna init
```

This will add _lerna_ to your `devDependencies` and create a `lerna.json`. The later will have some initial configuration. To use _yarn_ as package manager and make _lerna_ use workspaces, update the configuration like this:

```diff
{
-  "packages": [
-    "packages/*"
-  ],
+  "npmClient": "yarn",
+  "useWorkspaces": true,
  "version": "0.0.0"
}
```

The `packages` property is not longer needed, since we're reusing the configuration previously set in the `pacakge.json` (see ["Adding workspaces"](#adding_workspaces)).

I recommend to add _lerna_ to your npm scripts, so everyone is using the same version and does not have to install _lerna_ globally. In your root `package.json`:

```diff
{
  ...
  "scripts": {
+    "lerna": "lerna"
  }
}
```

With this script in place, you can alias all _lerna_ commands with `yarn lerna <command>`.

---

1. _<a name="footnote_1"></a> There is [one exception](https://github.com/yarnpkg/yarn/issues/4543), regarding binaries though._
