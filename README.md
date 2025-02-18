<a name="user-content-eslint-plugin-canonical"></a>
<a name="eslint-plugin-canonical"></a>
# eslint-plugin-canonical

[![NPM version](http://img.shields.io/npm/v/eslint-plugin-canonical.svg?style=flat-square)](https://www.npmjs.org/package/eslint-plugin-canonical)
[![Travis build status](http://img.shields.io/travis/gajus/eslint-plugin-canonical/master.svg?style=flat-square)](https://travis-ci.com/github/gajus/eslint-plugin-canonical)
[![js-canonical-style](https://img.shields.io/badge/code%20style-canonical-blue.svg?style=flat-square)](https://github.com/gajus/canonical)

ESLint rules for [Canonical ruleset](https://github.com/gajus/eslint-config-canonical).

<a name="user-content-eslint-plugin-canonical-installation"></a>
<a name="eslint-plugin-canonical-installation"></a>
## Installation

<!-- -->

```bash
npm install eslint --save-dev
npm install @typescript-eslint/parser --save-dev
npm install eslint-plugin-canonical --save-dev
```

<a name="user-content-eslint-plugin-canonical-configuration"></a>
<a name="eslint-plugin-canonical-configuration"></a>
## Configuration

1. Set `parser` property to `@typescript-eslint/parser`.
1. Add `plugins` section and specify `eslint-plugin-canonical` as a plugin.
1. Enable rules.

<!-- -->

```json
{
  "parser": "@typescript-eslint/parser",
  "plugins": [
    "canonical"
  ],
  "rules": {
    "canonical/filename-match-exported": 0,
    "canonical/filename-match-regex": 0,
    "canonical/filename-no-index": 0,
    "canonical/id-match": [
      2,
      "(^[A-Za-z]+(?:[A-Z][a-z]*)*\\d*$)|(^[A-Z]+(_[A-Z]+)*(_\\d$)*$)|(^(_|\\$)$)",
      {
        "ignoreDestructuring": true,
        "ignoreNamedImports": true,
        "onlyDeclarations": true,
        "properties": true
      }
    ],
    "canonical/no-restricted-strings": 0,
    "canonical/no-use-extend-native": 2,
    "canonical/prefer-inline-type-import": 2,
    "canonical/sort-keys": [
      2,
      "asc",
      {
        "caseSensitive": false,
        "natural": true
      }
    ]
  }
}
```

<a name="user-content-eslint-plugin-canonical-configuration-shareable-configurations"></a>
<a name="eslint-plugin-canonical-configuration-shareable-configurations"></a>
### Shareable configurations

<a name="user-content-eslint-plugin-canonical-configuration-shareable-configurations-recommended"></a>
<a name="eslint-plugin-canonical-configuration-shareable-configurations-recommended"></a>
#### Recommended

This plugin exports a [recommended configuration](./src/configs/recommended.json) that enforces Canonical type good practices.

To enable this configuration use the extends property in your `.eslintrc` config file:

```json
{
  "extends": [
    "plugin:canonical/recommended"
  ],
  "plugins": [
    "canonical"
  ]
}
```

See [ESLint documentation](https://eslint.org/docs/user-guide/configuring/configuration-files#extending-configuration-files) for more information about extending configuration files.

<a name="user-content-eslint-plugin-canonical-rules"></a>
<a name="eslint-plugin-canonical-rules"></a>
## Rules

<!-- Rules are sorted alphabetically. -->

<a name="user-content-eslint-plugin-canonical-rules-destructuring-property-newline"></a>
<a name="eslint-plugin-canonical-rules-destructuring-property-newline"></a>
### <code>destructuring-property-newline</code>

Like [`object-property-newline`](https://eslint.org/docs/rules/object-property-newline), but for destructuring.



<a name="user-content-eslint-plugin-canonical-rules-export-specifier-newline"></a>
<a name="eslint-plugin-canonical-rules-export-specifier-newline"></a>
### <code>export-specifier-newline</code>

Forces every export specifier to be on a new line.

Tip: Combine this rule with `object-curly-newline` to have every specifier on its own line.

```json
"object-curly-newline": [
  2,
  {
    "ExportDeclaration": "always"
  }
],
```

Working together, both rules will produces exports such as:

```ts
export { 
  a,
  b,
  c
};
```



<a name="user-content-eslint-plugin-canonical-rules-filename-match-exported"></a>
<a name="eslint-plugin-canonical-rules-filename-match-exported"></a>
### <code>filename-match-exported</code>

Match the file name against the default exported value in the module. Files that don't have a default export will be ignored. The exports of `index.js` are matched against their parent directory.

```js
// Considered problem only if the file isn't named foo.js or foo/index.js
export default function foo() {}

// Considered problem only if the file isn't named Foo.js or Foo/index.js
module.exports = class Foo() {}

// Considered problem only if the file isn't named someVariable.js or someVariable/index.js
module.exports = someVariable;

// Never considered a problem
export default { foo: "bar" };
```

If your filename policy doesn't quite match with your variable naming policy, you can add one or multiple transforms:

```json
"canonical/filename-match-exported": [ 2, "kebab" ]
```

Now, in your code:

```js
// Considered problem only if file isn't named variable-name.js or variable-name/index.js
export default function variableName;
```

Available transforms:

* snake
* kebab
* camel
* pascal

For multiple transforms simply specify an array like this (null in this case stands for no transform):

```json
"canonical/filename-match-exported": [2, [ null, "kebab", "snake" ] ]
```

If you prefer to use suffixes for your files (e.g. `Foo.react.js` for a React component file), you can use a second configuration parameter. It allows you to remove parts of a filename matching a regex pattern before transforming and matching against the export.

```json
"canonical/filename-match-exported": [ 2, null, "\\.react$" ]
```

Now, in your code:

```js
// Considered problem only if file isn't named variableName.react.js, variableName.js or variableName/index.js
export default function variableName;
```

If you also want to match exported function calls you can use the third option (a boolean flag).

```json
"canonical/filename-match-exported": [ 2, null, null, true ]
```

Now, in your code:

```js
// Considered problem only if file isn't named functionName.js or functionName/index.js
export default functionName();
```



<a name="user-content-eslint-plugin-canonical-rules-filename-match-regex"></a>
<a name="eslint-plugin-canonical-rules-filename-match-regex"></a>
### <code>filename-match-regex</code>

Enforce a certain file naming convention using a regular expression.

The convention can be configured using a regular expression (the default is `camelCase.js`). Additionally
exporting files can be ignored with a second configuration parameter.

```json
"canonical/filename-match-regex": [2, "^[a-z_]+$", true]
```

With these configuration options, `camelCase.js` will be reported as an error while `snake_case.js` will pass.
Additionally the files that have a named default export (according to the logic in the `match-exported` rule) will be
ignored.  They could be linted with the `match-exported` rule. Please note that exported function calls are not
respected in this case.



<a name="user-content-eslint-plugin-canonical-rules-filename-no-index"></a>
<a name="eslint-plugin-canonical-rules-filename-no-index"></a>
### <code>filename-no-index</code>

Having a bunch of `index.js` files can have negative influence on developer experience, e.g. when
opening files by name. When enabling this rule. `index.js` files will always be considered a problem.



<a name="user-content-eslint-plugin-canonical-rules-id-match"></a>
<a name="eslint-plugin-canonical-rules-id-match"></a>
### <code>id-match</code>

_The `--fix` option on the command line automatically fixes problems reported by this rule._

Note: This rule is equivalent to [`id-match`](https://eslint.org/docs/rules/id-match), except for addition of `ignoreNamedImports`.

This rule requires identifiers in assignments and `function` definitions to match a specified regular expression.

<a name="user-content-eslint-plugin-canonical-rules-id-match-options"></a>
<a name="eslint-plugin-canonical-rules-id-match-options"></a>
#### Options

* `"properties": false` (default) does not check object properties
* `"properties": true` requires object literal properties and member expression assignment properties to match the specified regular expression
* `"classFields": false` (default) does not class field names
* `"classFields": true` requires class field names to match the specified regular expression
* `"onlyDeclarations": false` (default) requires all variable names to match the specified regular expression
* `"onlyDeclarations": true` requires only `var`, `function`, and `class` declarations to match the specified regular expression
* `"ignoreDestructuring": false` (default) enforces `id-match` for destructured identifiers
* `"ignoreDestructuring": true` does not check destructured identifiers
* `"ignoreNamedImports": false` (default) enforces `id-match` for named imports
* `"ignoreNamedImports": true` does not check named imports



<a name="user-content-eslint-plugin-canonical-rules-import-specifier-newline"></a>
<a name="eslint-plugin-canonical-rules-import-specifier-newline"></a>
### <code>import-specifier-newline</code>

Forces every import specifier to be on a new line.

Tip: Combine this rule with `object-curly-newline` to have every specifier on its own line.

```json
"object-curly-newline": [
  2,
  {
    "ImportDeclaration": "always"
  }
],
```

Working together, both rules will produces imports such as:

```ts
import { 
  a,
  b,
  c
} from 'foo';
```



<a name="user-content-eslint-plugin-canonical-rules-no-restricted-strings"></a>
<a name="eslint-plugin-canonical-rules-no-restricted-strings"></a>
### <code>no-restricted-strings</code>

Disallow specified strings.


<a name="user-content-eslint-plugin-canonical-rules-no-restricted-strings-options-1"></a>
<a name="eslint-plugin-canonical-rules-no-restricted-strings-options-1"></a>
#### Options

The 1st option is an array of strings that cannot be contained in the codebase.

<a name="user-content-eslint-plugin-canonical-rules-no-use-extend-native"></a>
<a name="eslint-plugin-canonical-rules-no-use-extend-native"></a>
### <code>no-use-extend-native</code>



<a name="user-content-eslint-plugin-canonical-rules-prefer-inline-type-import"></a>
<a name="eslint-plugin-canonical-rules-prefer-inline-type-import"></a>
### <code>prefer-inline-type-import</code>

_The `--fix` option on the command line automatically fixes problems reported by this rule._

TypeScript 4.5 introduced [type modifiers](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-5.html#type-modifiers-on-import-names) that allow to inline type imports as opposed to having dedicated `import type`. This allows to remove duplicate type imports. This rule enforces use of import type modifiers.



<a name="user-content-eslint-plugin-canonical-rules-prefer-use-mount"></a>
<a name="eslint-plugin-canonical-rules-prefer-use-mount"></a>
### <code>prefer-use-mount</code>

In React, it is common to use [`useEffect`](https://reactjs.org/docs/hooks-reference.html#useeffect) without dependencies when the intent is to run the effect only once (on mount and unmount). However, just doing that may lead to undesired side-effects, such as the effect being called twice in [Strict Mode](https://reactjs.org/docs/strict-mode.html). For this reason, it is better to use an abstraction such as [`useMount`](https://stackoverflow.com/a/72319081/368691) that handles this use case.



<a name="user-content-eslint-plugin-canonical-rules-sort-keys"></a>
<a name="eslint-plugin-canonical-rules-sort-keys"></a>
### <code>sort-keys</code>

_The `--fix` option on the command line automatically fixes problems reported by this rule._

Note: This rule is equivalent to [`sort-keys`](https://eslint.org/docs/rules/sort-keys), except that it is fixable.

This rule requires identifiers in assignments and `function` definitions to match a specified regular expression.

<a name="user-content-eslint-plugin-canonical-rules-sort-keys-options-2"></a>
<a name="eslint-plugin-canonical-rules-sort-keys-options-2"></a>
#### Options

The 1st option is "asc" or "desc".

* "asc" (default) - enforce properties to be in ascending order.
* "desc" - enforce properties to be in descending order.

The 2nd option is an object which has 3 properties.

* `caseSensitive` - if true, enforce properties to be in case-sensitive order. Default is true.
* `minKeys` - Specifies the minimum number of keys that an object should have in order for the object's unsorted keys to produce an error. Default is 2, which means by default all objects with unsorted keys will result in lint errors.
* `natural` - if true, enforce properties to be in natural order. Default is false. Natural Order compares strings containing combination of letters and numbers in the way a human being would sort. It basically sorts numerically, instead of sorting alphabetically. So the number 10 comes after the number 3 in Natural Sorting.




<a name="user-content-eslint-plugin-canonical-faq"></a>
<a name="eslint-plugin-canonical-faq"></a>
## FAQ

<a name="user-content-eslint-plugin-canonical-faq-why-is-the-typescript-eslint-parser"></a>
<a name="eslint-plugin-canonical-faq-why-is-the-typescript-eslint-parser"></a>
### Why is the <code>@typescript-eslint/parser</code>?

This ESLint plugin is written using `@typescript-eslint/utils`, which assume that `@typescript-eslint/parser` is used.

Some rules may work without `@typescript-eslint/parser`. However, rules are implemented and tested assuming that `@typescript-eslint/parser` is used.