

#### About Linting and ESLint
JavaScript is a dynamic and loosely-typed language and as such, it is especially prone to subtle errors. Typically, a developer runs the code to find both syntax and logic errors. Linting tools like JSLint, JSHint and ESLint use a process know as static analysis to find syntax errors, and problematic code before you run the code. When integrated into you editor, linting allows you to find syntax errors while you type, like a spell-checker and grammar-checker for your code.

There are several linters available. JSLint, created by Douglas Crockford is the grandfather of Javascript linters. It is very opinionated and was very popular but it was not customizable. JHint was developed as an alternative to support customization but neither has not kept up with changes to Javascript. ESLint, developed by Nicholas C. Zakas is currently the most popular, customizable linter that supports modern Javascript syntax.

#### Install and Configure ESLint
ESLint can be installed and configured "globally" or "locally"
- When global, it is installed using the `--global` flag or `-g` for short like: `npm install -g eslint`. And configuration file is placed in your root directory so it will run across all projects. 
- When local, it is installed using with `--save-dev` option like `npm install eslint --save-dev` so an entry is placed in the devDependencies property in package.json. And a configuration file is placed in the root of the project which overrides the global configuration and allows for project specific configuration.

For now, we will install ESLint globally so it will be available for all your projects. 

First, from the Terminal (on a Mac) or Git-Bash (on windows) run the following command.

```bash
npm install -g eslint
```

That will install eslint in globally on your system and make it available on the command line.

Next, you'll create a eslint config file in your root directory. On the command line type `cd ~/` to navigate to your user root directory. Recall that the `~` symbol is shorthand for your user directory like `/Users/<YOUR-USERNAME>/`. Then create the file by typing `touch .eslintrc.json` on the command line and open the file in your favorite editor.

Copy and paste the following config into the `.eslintrc.json` file.

```
{
    /** 
    * ESLint: http://eslint.org/docs/user-guide/configuring
    */
    
    // "env:" supplies predefined global variables
    "env": {
        "browser": true,
        "es6": true,
        "node": true,
        "mocha": true,
        "mongo": true
    },
    // our configuration extends the recommended base configuration
    "extends": "eslint:recommended",
    // define the type of file `script` or `module` for ES6 Modules
    "parserOptions": {
        "sourceType": "script"
    },
    //ESLint rules: Severity Levels: off = 0 | warn = 1 | error = 2
    "rules": {
        "strict": ["error", "safe"],   //prefer `'use-strict';` pragma
        "eqeqeq":"error",              //prefer strict equality `===`
        "no-console": "warn",          //allows but warn about console like `console.log()`
        "no-unused-vars": "warn",      //warns about unused variables
        "no-eval": "error",            //disallows `eval()` usage
        "indent": ["error", 2],        //enforce 2 space indents (not tabs)        
        "quotes": ["error", "single"], //prefer single quotes over double quotes
        "semi": ["error", "always"]    //enforce semi-colon usage
    }
}
```

Let's discuss what the options mean:
- First, the ESLint JSON parser allows comments which are typically not allowed in JSON. This is why we can have non-standard comments in our config file.

- The `env` property tell eslint which types of environments you will be running. This sets up global variables such as `$` for jquery in the browser and `require` for node. You can find more information is the [Specifying Environments](http://eslint.org/docs/user-guide/configuring#specifying-environments) section of the documentation. 
- The `parserOptions` tells eslint how to parse the files. For Node development, we will use `script` which is the default. Also, setting `"sourceType": "script"` is required for the `'use strict';` pragma notification. Later in the course, you will create project specific configurations for React and set this to `module` which supports ES6 modules (`import` and `exports`);
- The `extends` property allows us to extend sets of pre-configure rules. Here we extend ESLint's base configuration. 
- ESLint supports 3 severity levels. Each can be defined as a string or a number value.
    - "off" or 0 - turn the rule off
    - "warn" or 1 - turn the rule on as a warning
    - "error" or 2 - turn the rule on as an error
- And is the `rules` property. Here we set some common defaults:
    - `"strict": ["error", "safe"]` prefer `'use-strict';` pragma
    - `"eqeqeq":"error"` prefer strict equality `===` (triple equals) over `==` (double equals)
    - `"no-console": "off"` allows but warns about console like `console.log()` which is appropriate for our course. But you will want this set to `"error"` when developing for production.
    - `"no-unused-vars"` warns about unused variables which is appropriate for our course because we will frequently define variables such as `(req, res)` but may only use `res`.
    - `"no-eval": "error"` disallows `eval()` usage
    - `"indent": ["error", 2]` Prefer 2 space indents and warns about indents of anything but 2 spaces. This also allows the `--fix` command to work properly. More on that later.
    - `"quotes": ["error", "single"]` Prefer single quotes and warns about using double quotes. This also allows the `--fix` command to work properly. More on that later.
    - `"semi": ["error", "always"]` Prefer semi-colons and warns if they are missing. This also allows the `--fix` command to work properly. More on that later.

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

