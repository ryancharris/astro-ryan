---
title: "How to configure ESLint and Prettier to work together"
publishDate: "2020-02-09"
published: true
layout: "../../layouts/BlogPost.astro"
description: "Learn how to use ESLint and Prettier to enforce code conventions and formatting."
category: "javascript"
tags:
  - "eslint"
  - "prettier"
---

Let's face it. Development is hard.

While developers have a multitude of concerns when building software, some of them simply aren't worth worrying about. In my mind, the enforcement of coding style and convention fall squarely within this category. As <a href="https://ryan.warner.codes/" target="_blank" >Ryan Warner</a> informed me, this is called <a href="https://whatis.techtarget.com/definition/Parkinsons-law-of-triviality-bikeshedding" target="_blank">bike-shedding</a>:

> ...an observation about the human tendency to devote a great deal of time to unimportant details while crucial matters go unattended.

Luckily, tools like <a href="https://eslint.org/" target="_blank">ESLint</a> and <a href="https://prettier.io/" target="_blank">Prettier</a> make it easier for developers to enforce their team's chosen coding standards. While having a bit of overlapping functionality, they're both aimed at improving different parts of the developer experience. However, configuring them to work together in harmony can be less that straightforward.

### ESLint

ESLint helps your team write consistent code in terms of style and syntax by enforcing the rules you define in your project's `my-repo/.eslintrc` configuration file. <a href="https://eslint.org/docs/rules/" target="_blank">For each rule</a>, you can choose to throw an error, log out a warning or ignore it altogether. Optionally, you can also have ESLint fix these violations based on your preferred configuration.

You can use ESLint in one of two ways: as <a href="https://eslint.org/docs/user-guide/" target="_blank">a CLI</a> or <a href="https://github.com/Microsoft/vscode-eslint" target="_blank">editor extension</a>.

You can use ESLint in a number of ways:

<ol>
  <li>
    <a href="https://eslint.org/docs/user-guide/" target="_blank">
      via the CLI
    </a>
  </li>
  <li>
    <a href="https://github.com/Microsoft/vscode-eslint" target="_blank">
      by installing an editor extension
    </a>
  </li>
  <li>
    <a href="https://webpack.js.org/loaders/eslint-loader/" target="_blank">
      using ESLint with a build tool
    </a>
  </li>
</ol>

The CLI comes in handy because you can lint (and fix) specific files and directories, use it in your pre-commit hooks, or integrate it into your CI/CD pipeline. Extensions, on the other hand, make your life easier when actually writing code by highlighting code blocks that break your preferred rules. In some cases, they can even suggest or fix violations on their own.

ESLint also allows you to extend rule sets, which are distributed as npm packages, from other developers and organizations. Extending a configuration like the one <a href="https://www.npmjs.com/package/eslint-config-airbnb" target="_blank">Airbnb's engineeering team</a> uses or <a href="https://github.com/standard/standard" target="_blank">StandardJS</a> is an easy way to get started with ESLint. Over time, if there are rules your team does not like, you can always override them to create a custom configuration.

### Prettier

Prettier allows you to automate your team's code formatting. This is a huge win because it completely eliminates formatting related comments from the review process. Like ESLint, Prettier uses a configuration file (i.e. `my-repo/.prettierrc`) to define the rules you want to enforce. Here's an example of what one would look like:

```json
{
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "jsxSingleQuote": false,
  "trailingComma": "all",
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "arrowParens": "avoid",
  "requirePragma": true,
  "insertPragma": true,
  "endOfLine": "lf"
}
```

Prettier also provides two similar tools to ESLint: <a href="https://prettier.io/docs/en/cli.html" target="_blank">a CLI</a> and <a href="https://github.com/prettier/prettier-vscode" target="_blank">editor extension</a>.

With the CLI, you can programmatically format files and/or directories within your repo. This saves a massive amount of time, especially when transitioning a project over to a new rule set.

The editor extension can be configured to auto-format the file you're working on when you save it. This ensures all of your commits are going to be properly formatted.

If you're using <a href="https://code.visualstudio.com/" target="_blank">VS Code</a>, you can add the following snippet to your user settings and let the robots do all the work! ???? If you're on a Mac, you can open your user settings with the `Cmd + ,` hotkey.

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "editor.formatOnSave": true
}
```

### ESLint && Prettier

When using ESLint and Prettier together, there are a couple packages you'll want to install in order to help them work well together.

1. <a href="https://www.npmjs.com/package/eslint-config-prettier" target="\_blank">eslint-config-prettier</a> - This disables ESLint's formatting rules and defers that concern to Prettier.

2. <a href="https://www.npmjs.com/package/eslint-plugin-prettier" target="\_blank">eslint-plugin-prettier</a> - Using this plugin allows ESLint to check for violations of Prettier rules and throw errors as part of its linting process.

3. <a href="https://www.npmjs.com/package/eslint-config-airbnb" target="_blank">eslint-config-airbnb</a> - Extending this configuration allows you to use Airbnb's preferred coding style and standards.

With these three packages installed, your `.eslintrc` would look something like this:

```json
{
  "plugins": ["prettier"],
  "extends": ["airbnb", "airbnb/hooks", "prettier"],
  "rules": {
    // Allow Prettier to throw errors via ESLint
    "prettier/prettier": "error"
  }
}
```

### Conclusion

By taking the time to configure ESLint and Prettier, you can automate away the concerns of formatting and code convention from your day to day work. This not only helps you but your team as well because it reduces cycles in the code review process.

Have a conversation with your team, agree upon a basic set of standards, and create some configuration files. Then, go forth and ship some well formmated code! ????
