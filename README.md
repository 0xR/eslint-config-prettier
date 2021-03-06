# eslint-config-prettier [![Build Status][travis-badge]][travis]

Turns all rules that are unnecessary or might conflict with [prettier] off.

This let’s you use you favorite shareable config without letting its stylistic
choices get in the way when using prettier.

Intended to be used together with [eslint-plugin-prettier].

## Installation

First, install [eslint-plugin-prettier]. Follow the instructions over there.

Then, install eslint-config-prettier:

```
$ npm install --save-dev eslint-config-prettier
```

Finally, add eslint-config-prettier to the "extends" array in your `.eslintrc.*`
file. Make sure to put it **last,** so it gets the chance to override other
configs.

```json
{
  "extends": [
    "prettier"
  ]
}
```

A few ESLint plugins are supported as well:

- [eslint-plugin-flowtype]
- [eslint-plugin-react]

Add extra exclusions for the plugins you use like so:

```json
{
  "extends": [
    "prettier",
    "prettier/flowtype",
    "prettier/react"
  ]
}
```

## CLI helper tool

eslint-config-prettier also ships with a little CLI tool to help you check if
your configuration contains any rules that are unnecessary or conflict with
prettier.

First, add a script for it to package.json:

```json
{
  "scripts": {
    "eslint-check": "eslint --print-config .eslintrc.js | eslint-config-prettier-check"
  }
}
```

Then run `npm run eslint-check`.

(Swap out .eslintrc.js with the path to your config if needed.)

## Example configuration

```json
{
  "extends": [
    "google",
    "plugin:flowtype/recommended",
    "plugin:react/recommended",
    "prettier",
    "prettier/flowtype",
    "prettier/react"
  ],
  "plugins": [
    "flowtype",
    "react",
    "prettier"
  ],
  "parserOptions": {
    "ecmaVersion": 2016,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "env": {
    "es6": true,
    "node": true
  },
  "rules": {
    "prettier/prettier": "error"
  }
}
```

## Special rules

There a few rules that eslint-config-prettier disables that actually can be
enabled in some cases.

### [max-len]

Usually, prettier takes care of following a maximum line length automatically.
However, there are cases where prettier can’t do anything, such as for long
strings, regular expressions and comments. Those need to be split up by a human.

If you’d like to enforce an even stricter maximum line length policy than
prettier can provide automatically, you can enable this rule. Just remember to
keep `max-len`’s options and prettier’s `printWidth` option in sync.

Example configuration:

```json
{
  "rules": {
    "max-len": ["error", {"code": 80, "ignoreUrls": true}]
  }
}
```

### [no-mixed-operators]

This rule forbids mixing certain operators, such as `&&` and `||`.

For example, the rule could warn about this line:

```js
var foo = a && b || c;
```

The rule suggests adding parentheses, like this:

```js
var foo = (a && b) || c;
```

However, prettier prints the minimum amount of parentheses technically needed,
turning it back to:

```js
var foo = a && b || c;
```

If you want to use this rule with prettier, you need to split the expression
into another variable:

```js
var bar = a && b;
var foo = bar || c;
```

Example configuration:

```json
{
  "rules": {
    "no-mixed-operators": "error"
  }
}
```

### [quotes]

If you’d like to enforce the use of backticks rather than single or double
quotes for strings, you can enable this rule. Otherwise, there’s no need to.
Just remember to enable the `"backtick"` option!

Example configuration:

```json
{
  "rules": {
    "quotes": ["error", "backtick"]
  }
}
```

## Contributing

eslint-config-prettier has been tested with:

- ESLint 3.15.0
- prettier 0.16.0
- eslint-plugin-flowtype 2.30.0
- eslint-plugin-react 6.9.0

Have new rules been added since those versions? Have we missed any rules? Is
there a plugin you would like to see exclusions for? Open an issue or a pull
request!

If you’d like to add support for eslint-plugin-foobar, this is how you’d go
about it:

First, create `foobar.js`:

```js
"use strict";

module.exports = {
  rules: {
    "foobar/some-rule": "off"
  }
};
```

Then, create `test-lint/foobar.js`:

```js
/* eslint-disable quotes */
"use strict";

// prettier does not want spaces before the parentheses, but
// eslint-config-foobar wants one.
console.log ();
```

`test-lint/foobar.js` must fail when used with eslint-plugin-foobar and
eslint-plugin-prettier at the same time – until `"prettier/foobar"` is added to
the "extends" property of an ESLint config.

Finally, you need to mention the plugin in several places:

- Add `"foobar.js"` to the "files" field in `package.json`.
- Make sure that at least one rule from eslint-plugin-foobar gets used in
  `.eslintrc.base.js`.
- Add it to the list of supported plugins, to the example config and to
  Contributing section in `README.md`.

When you’re done, run `npm test` to verify that you got it all right. It runs
several other npm scripts:

- `"test:lint"` makes sure that the files in `test-lint/` pass ESLint when
  the exclusions from eslint-config-prettier are used. It also lints the code of
  eslint-config-prettier itself.
- `"test:lint-verify-fail"` is run by a test in `test/lint-verify-fail.js`.
- `"test:lint-rules"` is run by a test in `test/rules.js`.
- `"test:ava"` runs unit tests that check a number of things:
  - That eslint-plugin-foobar is mentioned in all the places shown above.
  - That no unknown rules are turned off. This helps catching typos, for
    example.
  - That the CLI works.
- `"test:cli-sanity"` is a sanity check for the CLI.

## License

[MIT](LICENSE).

[eslint-plugin-flowtype]: https://github.com/gajus/eslint-plugin-flowtype
[eslint-plugin-prettier]: https://github.com/not-an-aardvark/eslint-plugin-prettier
[eslint-plugin-react]: https://github.com/yannickcr/eslint-plugin-react
[max-len]: http://eslint.org/docs/rules/max-len
[no-mixed-operators]: http://eslint.org/docs/rules/no-mixed-operators
[prettier]: https://github.com/jlongster/prettier
[quotes]: http://eslint.org/docs/rules/quotes
[travis-badge]: https://travis-ci.org/lydell/eslint-config-prettier.svg?branch=master
[travis]: https://travis-ci.org/lydell/eslint-config-prettier
