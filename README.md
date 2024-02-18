# Project configuration for the _Quality Gates (static code analysis)_

The documentation shows how to prepare an automated test project for static code analysis and code formatting using Prettier and ESLint

## QA
- [Dominik Calak](https://github.com/DominikCLK)

<p align="center">
  <a href="#">
    <img src="https://simpleskill.icons.workers.dev/svg?i=visualstudiocode,node.js,eslint,playwright,typescript,prettier" />
  </a>
</p>

<br>

1. [Playwright Test installation, extensions and settings](#1)
2. [Prettier settings](#2)
3. [ESLint setup](#3)
4. [ESLint setup for Playwright](#4)
5. [ESLint + Prettier setup](#5)
6. [Import management](#6)
7. [Final settings](#7)
8. [Adds scripts in _package.json_](#8)
9. [Husky](#9)


<br>

# 1. Playwright Test installation, extensions and settings <a id="1"></a>

<br>

- Install the extension “Playwright Test for VSCode”

![2024-02-16_13h57_46](https://github.com/DominikCLK/Wiki-for-Project-configuration-for-the-Quality-Gates-static-code-analysis/assets/75272795/f8610f17-333c-4245-b08b-be7c14e9bba2)

<br>
 
- Add the Playwright skeleton to the project

>- use “Ctrl Shift P”
>- type and choose the action “Test: Install Playwright”
>- optionally, leave only the check mark for the Chromium browser, click Ok

- Install more extension like:
>- Prettier
>- Code Spell Checker
>- Material Icons Theme
>- ESlint

<br>

- VSC settings
 >- create .vscode folder
 >- into .vscode folder create _settings.json_ file with settings:


```
{
 "editor.formatOnSave": true,
 "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```
Enable two options: formatting on save and Prettier as the default formatter.

>- go File -> Auto Save
>- click “right” -> Format Document With.. -> Prettier - Code formatter


# 2. Taking care of code quality

Prettier: formatting various files to assumed standards.
ESLint: indicating errors in the code of JS/TS files.

The areas of both tools overlap, so they need to be connected.

<br>

## 1. Prettier settings <a id="2"></a>

>- Open Terminal and type command:

```
npm install --save-dev  --save-exact prettier
```

>- Create .prettierignore file with settings:

```
package-lock.json
playwright-report
test-results
```

>- Create .prettierrc.json file and set settings:

```
{
 "singleQuote": true,
 "endOfLine": "auto",
 "tabWidth": 2,
 "semi": true
}
```

<br>

## 2. ESLint setup <a id="3"></a>

>- Install ESLint

```
npm install eslint --save-dev
```
Configure ESLint

```
npm init @eslint/config
```

- Then follow rules:

```
? How would you like to use ESLint?
 >To check syntax and find problems
? What type of modules does your project use?
 >JavaScript modules (import/export)
? Which framework does your project use?
 >None of these
? Does your project use TypeScript? 
 >Yes
? Where does your code run?
 > (click " i ") Node
? What format do you want your config file to be in?
 >JSON
? Would you like to install them now?
 > Yes
? Which package manager do you want to use?
 >npm
```

- Then you will see new file _.eslintrc.json_

<br>

## 3. ESLint setup for Playwright <a id="4"></a>
```
npm install eslint-plugin-playwright --save-dev
```

- add this package to extends in _.eslintrc.json_ file

```
 "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:playwright/playwright-test",
  ]
```

<br>

## 4. ESLint + Prettier setup <a id="5"></a>

- Install package (Resolving conflicts between Prettier and ESLint)

```
npm install eslint-config-prettier --save-dev
```

- Running Prettier as a rule for ESLint

``` 
npm install eslint-plugin-prettier@latest --save-dev
```

- add prettier package to extends and plugins in .eslintrc.json file

```
 "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:playwright/playwright-test",
    "prettier"
  ],

},
 "plugins": ["@typescript-eslint", "prettier"],
```

- Create _.eslintignore_ file

```
package-lock.json
playwright-report
test-results
```
- run ESLint

```
npx eslint . --ext .ts --max-warnings=0
```

- on parserOptions ( _.eslintrc.json_ ) list add

```
"warnOnUnsupportedTypeScriptVersion": false
```

- on rules ( _.eslintrc.json_ ) list add

```
 "rules": {
    "prettier/prettier": "warn",
    "@typescript-eslint/explicit-function-return-type": "error",
   "no-console": "warn",
}
```

<br>

# 3. Import management <a id="6"></a>

<br>

- install package

```
npm install --save-dev @trivago/prettier-plugin-sort-imports
```

- In the Prettier configuration _.prettierrc.json_

```
   "importOrderSeparation": true,
    "importOrderSortSpecifiers": true,
    "plugins": ["@trivago/prettier-plugin-sort-imports"]
```

- Setting to remove unused imports when saving to VSC: _.vscode/settings.json_

```
   "editor.codeActionsOnSave": {
        "source.organizeImports": "explicit"
}
```

- add to the _.prettierrc.json_ file

```
"plugins": ["@trivago/prettier-plugin-sort-imports"]
```
- add to _settings.json_

```
"cSpell.words": [
"trivago",
]
```

<br>

# 4. Final settings <a id="7"></a>


- .vscode > settings.json

```
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "cSpell.words": ["trivago"],
  "editor.codeActionsOnSave": {
    "source.organizeImports": "explicit"
  }
}
```

- .prettierrc.json

```
{
 "singleQuote": true,
 "endOfLine": "auto",
 "tabWidth": 2,
 "semi": true,
 "importOrderSeparation": true,
 "importOrderSortSpecifiers": true
  "plugins": ["@trivago/prettier-plugin-sort-imports"]
}
```

- .eslintrc.json

```
{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:playwright/playwright-test",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module",
    "warnOnUnsupportedTypeScriptVersion": false
  },
  "plugins": ["@typescript-eslint", "prettier"],
  "rules": {
    "@typescript-eslint/explicit-function-return-type": "error",
    "no-console": "warn",
    "prettier/prettier": "warn"
  }
}
```

<br>


# 5. Adds scripts in _package.json_ <a id="8"></a>

```
  "scripts": {
    "format": "npx prettier --write .",
    "format:check": "npx prettier . --check \"!**.ts\"",
  "lint": "npx eslint . --ext .ts --max-warnings=0",
  },
```

<br>

# 5. Husky  <a id="9"></a>
Automated code verification before commit. Protecting the framework against bugs.

## 1. Preparing Husky
   
- Install Husky package
  
```
npm install husky --save-dev
```

- Install Husky in project

```
npx husky init
```

- Adding a linting command to Husky before the commit action

```
echo "npm run lint" > .husky/pre-commit
```

- Configuration verification: _.husky folder_, _pre-commit_ file

```
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm run lint
```

























