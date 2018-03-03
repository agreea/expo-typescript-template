# Demo Expo x Typescript Project

Expo allows you to quickly build and deploy React Native apps. It allows developers to lever native features, updates over the air, and handles publishing apps to the app store. The React Native team has worked with Expo to make getting a React Native app setup a breeze via [`create-react-native-app`](https://github.com/react-community/create-react-native-app).

Despite all of the great power that Expo unlocks for developers, it doesn't provide an easy out-of-the-box way to develop your React Native app in Typescript. This project walks through setting up Typescript and Expo from scratch. Together, they make mobile development much faster and less bug-prone.

## Prerequisites
1. [Install Expo and dependencies](https://docs.expo.io/versions/latest/introduction/installation.html).
2. [Install Typescript](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html).
3. [Set up Expo](https://docs.expo.io/versions/latest/guides/up-and-running.html).

Much of this document is borrowed from janaagaard75's [`expo-and-typescript`](https://github.com/janaagaard75/expo-and-typescript).

## Setting up Typescript with Expo

Add Typescript and the helper library, `tslib`, to your project:

```
yarn add --dev typescript react-native-typescript-transformer
yarn add tslib
```

Add `tsconfig.json` to the project root to configure Typescript:
*TODO: Not all these are necessary, clean this up.*
```json
{
  "compilerOptions": {
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "forceConsistentCasingInFileNames": true,
    "importHelpers": true,
    "jsx": "react-native",
    "lib": [
      "dom",
      "es2015",
      "es2016",
      "es2017"
    ],
    "module": "es2015",
    "moduleResolution": "node",
    "noEmitHelpers": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "outDir": "build/dist",
    "sourceMap": true,
    "strict": true,
    "target": "es2017"
  },
  "exclude": [
    "build",
    "node_modules"
  ],
  "types": [
    "typePatches"
  ]
}
```

Add the React Native TypeScript Transformer package.

```shell
yarn add --dev --exact react-native-typescript-transformer
```

Configure Expo to use the transformer for `ts` and `tsx` files by adding the following lines to `app.json` under `expo/packagerOpts`. [The final app.json should look somewhat like this](https://raw.githubusercontent.com/janaagaard75/expo-and-typescript/master/app.json).

```json
"sourceExts": [
  "ts",
  "tsx"
],
"transformer": "node_modules/react-native-typescript-transformer/index.js"
```


### App Component in TypeScript

Create a `src` folder, move `App.js` to that folder, and rename the file to `App.tsx`. Since TypeScript has a syntax that is so similar to JavaScript it's not necessary to make any modifications to App.tsx to make it valid TypeScript.

Create a new `App.js` in the root of the project, and insert the following lines. Expo will still be looking for App.js in the root of the project, so we simply tell it to use the new `src/App.tsx.

```javascript
import App from './src/App'

export default App
```

### Add Type Definitions for React

Add type definitions for React and React Native.

```shell
yarn add --dev --exact @types/react @types/react-native
```

### Add Type Definitions for Expo

Besides what React Native already has, the Expo SDK comes with a lot of additional APIs for your app. Unfortunately there aren't any type definitions for these APIs, and that makes it difficult to use them correctly in TypeScript. I have started on creating these type definitions, but bare in mind that they still lack a lot of testing.

Create a file `expo.d.ts` in the `src` folder and copy the content of this [`expo.d.ts`](https://raw.githubusercontent.com/janaagaard75/expo-sdk-with-type-definitions/master/expo.d.ts) to it.

### Reloading the app
Sources online report that TS + Expo's hot reloading is less reliable than Expo with plain JS. Hot reloading _should_ work with TS if you open a terminal tab and run `tsc -w` in the root directory of the project.