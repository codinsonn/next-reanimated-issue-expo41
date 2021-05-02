# Minimal Repro of Issues with @expo/next-adapter

Steps to reproduce 👇

## 1. Init new expo project with TS template 

```bash
$ expo init -t expo-template-blank-typescript
```

## 2. Add @expo/next-adapter

```bash
$ yarn add -D @expo/next-adapter
$ yarn next-expo -c -f
```

<details>
    <summary>Show -c chosen options</summary>

    // TODO: Add screenshot
</details>

## 3. Run `next dev` to update initial tsconfig.json

```bash
$ yarn dev
```

> This causes a first (minor) issue where `@babel/plugin-proposal-class-properties` is missing

<details>
    <summary>Show error screenshot</summary>
    
    // TODO: Add screenshot
</details>

This is, ofcourse, easily fixed by adding the babel plugin manually in the next step
(but maybe a good future addition to the base `babel.config.js` resulting from `yarn next-expo`?)

## 4. Add missing `@babel/plugin-proposal-class-properties`

```bash
expo install @babel/plugin-proposal-class-properties
```

<details>
    <summary>Edited `babel.config.js`</summary>

```js
// @generated: @expo/next-adapter@2.1.69
// Learn more: https://github.com/expo/expo/blob/master/docs/pages/versions/unversioned/guides/using-nextjs.md#shared-steps

module.exports = { presets: ['@expo/next-adapter/babel'], plugins: ['@babel/plugin-proposal-class-properties'] };
```
</details>

This solves issue 1.

## 5. Rename pages from `.js` to `.ts`

```bash
mv pages/_document.js pages/_document.ts
mv pages/index.js pages/index.ts
```

This causes a second build issue in `pages/index.ts`: `Syntax error: Unexpected token, expected ","`

<details>
    <summary>Show error screenshots</summary>
    // TODO: Add screenshots
</details>

> The issue might be with next itself in this case, but it does come as a surprise since AFAIK typescript is supported in both next & expo, and next-adapter provides react-native-web support

<details>
    <summary>Show `tsconfig.json` at this point</summary>

```json
{
  "extends": "expo/tsconfig.base",
  "compilerOptions": {
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "isolatedModules": true,
    "jsx": "preserve"
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx"
  ]
}
```
</details>
