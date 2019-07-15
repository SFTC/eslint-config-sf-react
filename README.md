# eslint-config-sf-react

This package includes the shareable ESLint configuration used by [SF-Tech](https://github.com/babyisun/eslint-config-sf-react).<br>
Please refer to its documentation:

## Usage

If you want to use this ESLint configuration in your project, you can install it with the following steps.

First, install this package, ESLint and the necessary plugins.

```sh
npm install --save-dev eslint-config-sf-react babel-eslint@9.x eslint@5.x eslint-plugin-flowtype@2.x eslint-plugin-import@2.x eslint-plugin-jsx-a11y@6.x eslint-plugin-react@7.x
```

or

```sh
yarn add --dev eslint-config-sf-react babel-eslint@9.x eslint@5.x eslint-plugin-flowtype@2.x eslint-plugin-import@2.x eslint-plugin-jsx-a11y@6.x eslint-plugin-react@7.x
```

Then create a file named `.eslintrc` with following contents in the root folder of your project:

```json
{
  "extends": "sf-react"
}
```

If you want, you can also create a file named `.prettierrc` with following contents in the root folder of your project:

```json
{
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "all",
  "jsxBracketSameLine": false,
  "endOfLine": "lf",
  "jsxSingleQuote": true
}
```

That's it! You can override the settings from `eslint-config-sf-react` by editing the `.eslintrc` file. Learn more about [configuring ESLint](http://eslint.org/docs/user-guide/configuring) on the ESLint website.

If you want to enable even more accessibility rules, you can create an `.eslintrc` file in the root of your project with this content:

```json
{
  "extends": ["sf-js", "sf-react", "sf-vue", "sf-ts", "sf-mp"],
  "plugins": ["sf-plugin"]
}
```

## Get more
[React detailed rules](./SF-REACT-RULES.md)
