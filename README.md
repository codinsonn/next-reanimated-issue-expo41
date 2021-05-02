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

![chosen next-expo options](https://user-images.githubusercontent.com/5967956/116798187-5a118200-aaed-11eb-94fa-c155d4091904.png "Screenshot 2021-05-02 at 02 16 22")

</details>

## 3. Run `next dev` to update initial tsconfig.json

```bash
$ yarn dev
```

> This causes a first (minor) issue where `@babel/plugin-proposal-class-properties` is missing

<details>
    <summary>Show error screenshot</summary>
    
<img width="1238" alt="Screenshot 2021-05-02 at 01 32 01" src="https://user-images.githubusercontent.com/5967956/116798195-744b6000-aaed-11eb-9cc2-70cec890c2db.png">

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
mv pages/_document.js pages/_document.tsx
mv pages/index.js pages/index.tsx
```
