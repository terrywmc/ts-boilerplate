# Setup a Node.js project with Typescript, ESLint, Prettier, Husky, Lint-staged, Jest

> Starting a node project with:

- Typescript hot reload during development
- Testing compatibilty
- Auto linting and formating when commit

## Start a project

We use pnpm in this example

```shell
pnpm init -y
```

```shell
git init
```

Add `.gitignore` with

```
node_modules
dist
coverage
.husky
```

## Typescript

```shell
pnpm add typescript --D
```

Create a `ts.config.json` file under the root directory with

```json
{
  "compilerOptions": {
    "target": "es2022",
    "module": "commonjs",
    "outDir": "dist",
    "sourceMap": true
  },
  "include": ["src/**/*.ts"]
}
```

Adding the following line to package.json for compile ts to js

```json
{
  "scripts": {
    "build": "tsc"
  }
}
```

Then, un the command in shell

```shell
pnpm build
```

## TS-node

To run ts file directly, we need ts-node

```shell
pnpm add ts-node @types/node --save-dev
```

Adding the following line to package.json

```json
{
  "scripts": {
    "start": "ts-node src/index.ts"
  }
}
```

To start the app, run this

```shell
pnpm start
```

## Nodemon

Nodemon watch the file chnanges for hot reloading in development

```shell
pnpm add nodemon --save-dev
```

Then, create a `.eslintrc` file to add configuration as follows

```json
{
  "watch": ["src"],
  "ext": "ts,json",
  "ignore": ["src/**/*.spec.ts"],
  "exec": "ts-node ./src/index.ts"
}
```

Adding the following line to package.json

```json
{
  "scripts": {
    "dev": "nodemon"
  }
}
```

To start the dev mode, run this

```shell
pnpm dev
```

## ESLint

ESlint keep the consistency and productiving of coding, and it's compatible with lots of plugins and styles.

To install the typescript parser and and plugin

```shell
pnpm add eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser --save-dev
```

Then, create a `.eslintrc` file to add configuration as follows

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": { "ecmaVersion": 2022, "sourceType": "module" },
  "plugins": ["@typescript-eslint"],
  "extends": ["plugin:@typescript-eslint/recommended"],
  "env": {
    "browser": true,
    "es2021": true
  },
  "rules": {}
}
```

Then, add the following lines in `package.json`:

```json
{
  "scripts": {
    "lint": "eslint src/**/*.ts",
    "format": "eslint src/**/*.ts --fix"
  }
}
```

To lint, run this

```shell
pnpm lint
```

To format your file with linting rules, run this in shell

```shell
pnpm format
```

## Prettier

Prettier helps changing the structural view with simple rules

```shell
pnpm add prettier --save-dev
```

Add a `.prettierrc` to the root of the project directory

```json
{
  "semi": true,
  "trailingComma": "all",
  "singleQuote": true,
  "printWidth": 120,
  "tabWidth": 2
}
```

Prettier can integrate with Visual Studio Code easily, press `Ctrl + Shift + P` and then select `Format Document`, or enable `Format on Save` in settings.

To combine Prettier with ESlint

```shell
pnpm add eslint-config-prettier eslint-plugin-prettier --save-dev
```

Then, add Prettier as extends to eslint config file `.eslintrc` P

```js
  // HERE
  "plugins": [..., "prettier"],
  "extends": [..., "prettier"],
  ...
  "rules": {
    ...,
    "prettier/prettier": "error"
  }
```

To format your file with linting rules, run this in shell

```shell
pnpm format
```

## Husky & Lint-staged

Husky helps in auto linting before commiting the code, and lint-staged

```
pnpm add husky lint-staged --save-dev
npx mrm lint-staged
```

Running `npx mrm lint-staged` will create `.husky` folder and add config into package.json:

```json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

Update the lint-staged section in the package.json

```json
{
  "lint-staged": {
    "*.ts": "eslint --cache --fix"
  }
}
```

It wll automatically fix the `git commit` when staged code get error in linting

## Jest

Install Jest and ts-jest by

```shell
pnpm add jest ts-jest @types/jest --save-dev
```

Creating config file `jest.config.js` by

```shell
pnpm ts-jest config:init
```

The config file will look like:

```js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
};
```

Or we can add its config into package.json:

```json
{
  // [...]
  "jest": {
    // Replace `ts-jest` with the preset you want to use
    // from the above list
    "preset": "ts-jest"
  }
}
```

Then, add the following lines in `package.json`:

```json
{
  "scripts": {
    "test": "jest --coverage"
  }
}
```