

#### About Linting and ESLint
JavaScript is a dynamic and loosely-typed language and as such, it is especially prone to subtle errors. Typically, a developer runs the code to find both syntax and logic errors. Linting tools like JSLint, JSHint and ESLint use a process know as static analysis to find syntax errors, and problematic code before you run the code. When integrated into you editor, linting allows you to find syntax errors while you type, like a spell-checker and grammar-checker for your code.

There are several linters available. JSLint, created by Douglas Crockford is the grandfather of Javascript linters. It is very opinionated and was very popular but it was not customizable. JHint was developed as an alternative to support customization but neither has not kept up with changes to Javascript. ESLint, developed by Nicholas C. Zakas is currently the most popular, customizable linter that supports modern Javascript syntax.

#### Install and Configure ESLint
ESLint can be installed "globally" and configured at the root so it will run across all your projects. It can also be installed and configured "locally", that is for each project which allows for project specific configuration. For now, we will install ESLint globally so it will be available for all your projects. 

First, from the command line (Git-Bash if you're on windows), `cd ~/` to ensure you are in your user root directory and then install ESLint globally by running `npm install -g eslint`.

Next, run `eslint --init`. You will be prompted to enter your preferences. Use the arrow keys and space bar to select the following preferences.

```text
eslint --init

? **How would you like to configure ESLint?**
=> Answer questions about your style
Use a popular style guide
Inspect your JavaScript file(s)

? Are you using ECMAScript 6 features? (y/N) Yes

? Are you using ES6 modules? (y/N) Yes

? **Do you use CommonJS?** (y/N) No

? **Where will your code run?**
◉ Browser
◉ Node

? **Do you use JSX?** (y/N) No

? **What style of indentation do you use?** (Use arrow keys)
Tabs
=> Spaces

? **What quotes do you use for strings?**
Double
=> Single

? **What line endings do you use?** (Use arrow keys)
[On Mac and Linux, choose Unix. On Windows, choose Windows]

? **Do you require semicolons?** (Y/n) Yes

? **What format do you want your config file to be in?** (Use arrow keys)
JavaScript
YAML
=> JSON
```

If you get the following error, you chose to "Y" for JSX and React. Please re-run `eslint --init` and choose "N" for JSX. 
```
Could not find a package.json file. Run 'npm init' to create one.
Error: Could not find a package.json file. Run 'npm init' to create one.
    at check (/usr/local/lib/node_modules/eslint/lib/util/npm-util.js:76:15)
    at Object.checkDevDeps (/usr/local/lib/node_modules/eslint/lib/util/npm-util.js:124:12)
...
```

> Note, we will configure ESLint to use JSX and React on a per-project basis during the React phase of the course.

Open the config file in your editor. If you setup VS Code per instructions simple enter `code ~/.eslintrc.js` in the Terminal. Make the following changes to the `rules` property.

- Add: `"eqeqeq":"error",`
- Add: `"no-console": "warn",`
- Add: `"no-eval": "error",`
- And change the indent rule from 4 to 2.

Your config file should look like the one below.

```JSON
{
    "env": {
        "browser": true,
        "es6": true,
        "node": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "sourceType": "module"
    },
    "rules": {
        // Add these rules
        "eqeqeq":"error",
        "no-console": "warn",
        "no-eval": "error",

        // Change indent from 4 to 2 spaces
        "indent": [
            "error",
            2
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "single"
        ],
        "semi": [
            "error",
            "always"
        ]
    }
}
```


#### ESLint in the Terminal
You can now lint files from the command line. Let give it a try.

In the Terminal, navigate to a recent project and type `eslint filename.js`. If you file has any errors, you'll see output similar to the following.

```
eslint filename.js

/Users/sallystudent/my-project/index.js
  39:7   error  Unexpected console statement  no-console
  42:18  error  Missing semicolon             semi
  52:5   error  Unexpected console statement  no-console
  67:28  error  Unexpected console statement  no-console
  68:2   error  Unnecessary semicolon         no-extra-semi

✖ 5 problems (5 errors, 0 warnings)

```

Notice the first column. In the example above it says `39:7`. This refers to Line 39 column 7. Open your project in your editor and fix the error then re-run `eslint filename.js` and save the file. You should see the error removed from the list.

ESLint offers a special feature that will auto correct some simple mistakes like spacing and missing semi-colons. Back in your terminal, type `eslint --fix filename.js`. After auto-fixing the errors, the command will output a new list of results.

```js
`eslint --fix filename.js`

/Users/sallystudent/my-project/index.js
  39:7   error  Unexpected console statement  no-console
  52:5   error  Unexpected console statement  no-console
  67:28  error  Unexpected console statement  no-console

✖ 3 problems (3 errors, 0 warnings)
```

#### VSCode setup
Open Visual Studio Code and click on the Extensions icon in the left navigation bar or use the shortcut (Shift + Command + X). A slide-out panel will open and at the top is a "Search Extensions in Marketplace" input. Type "ESLint", you will see a few results. Install the one "ESLint" by Dirk Baeumer and then click "Reload". Now navigate to your project and open a javascript file.

If there are any errors, your see a red squiggly line under the code. Hover your mouse over the code and a small pop-up will appear describing the error. For instance `[eslint] Missing semicolon. (semi)`. 

![eslint-onhover-error](eslint-onhover-error.png)

Hightlight the code with the error, if the error is auto-fixable you'll see a small lightbulb in the left-hand gutter. Click the lightbulb to see a list of options like `Fix this semi problem` and `Fix all auto-fixable problems`.


![eslint-fix-this-semi-problem](eslint-fix-this-semi-problem.png)


For more information checkout the [ESLint Extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) description on the Marketplace. And [ESLint Documentation](http://eslint.org/) 

