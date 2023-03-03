# Steps to create react library

1. Create a folder where to save the library
2. Run the next command for install react dependencies:

```bash
yarn add -D react react-dom typescript @types/react
```

or with npm:

```bash
npm install --save-dev react react-dom typescript @types/react
```

3. Create tsconfig,json file with the next command:

```bash
npx tsc --init
```

4. Copy and paste the next configuration into tsconfig.json

```json
{
  // see https://www.typescriptlang.org/tsconfig to better understand tsconfigs
  "include": ["src", "types"],
  "compilerOptions": {
    "rootDir": "src",
    "outDir": "dist",
    "module": "esnext",
    "lib": ["dom", "esnext"],
    "importHelpers": true,
    // output .d.ts declaration files for consumers
    "declaration": true,
    // output .js.map sourcemap files for consumers
    "sourceMap": true,
    // match output dir to input dir. e.g. dist/index instead of dist/src/index
    // stricter type-checking for stronger correctness. Recommended by TS
    "strict": true,
    // linter checks for common issues
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    // noUnused* overlap with @typescript-eslint/no-unused-vars, can disable if duplicative
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    // use Node's module resolution algorithm, instead of the legacy TS one
    "moduleResolution": "node",
    // transpile JSX to React.createElement
    "jsx": "react",
    // interop between ESM and CJS modules. Recommended by TS
    "esModuleInterop": true,
    // significant perf increase by skipping checking .d.ts files, particularly those in node_modules. Recommended by TS
    "skipLibCheck": true,
    // error out if import and file system have a casing mismatch. Recommended by TS
    "forceConsistentCasingInFileNames": true,
    "noEmit": true
  }
}
```

5. Install Rollup, if you don't want to add css remove these libraries`rollup-plugin-postcss`and `postcss`:

```bash
yarn add -D rollup @rollup/plugin-commonjs @rollup/plugin-node-resolve @rollup/plugin-terser @rollup/plugin-typescript rollup-plugin-dts rollup-plugin-peer-deps-external rollup-plugin-terser tslib rollup-plugin-postcss postcss
```

6. Create the file rollup.config.mjs and paste this:

```js
import resolve from "@rollup/plugin-node-resolve";
import commonjs from "@rollup/plugin-commonjs";
import typescript from "@rollup/plugin-typescript";
//Optional with css
import postcss from "rollup-plugin-postcss";
import dts from "rollup-plugin-dts";
import { terser } from "rollup-plugin-terser";
import peerDepsExternal from "rollup-plugin-peer-deps-external";
import packageJson from "./package.json" assert { type: "json" };

export default [
  {
    input: "src/index.ts",
    output: [
      {
        file: packageJson.main,
        format: "cjs",
        sourcemap: true,
      },
      {
        file: packageJson.module,
        format: "esm",
        sourcemap: true,
      },
    ],
    plugins: [
      peerDepsExternal(),
      resolve(),
      commonjs(),
      typescript({ tsconfig: "./tsconfig.json" }),
      //Optional With css
      postcss({
        modules: true,
      }),
      terser(),
    ],
  },
  {
    input: "dist/esm/index.d.ts",
    output: [{ file: "dist/index.d.ts", format: "esm" }],
    plugins: [dts()],
    external: [/\.css$/],
  },
];
```

## if you want install jest follow the next steps:
1. Install jest and another esencial libraries

```bash
@testing-library/jest-dom @testing-library/react @testing-library/user-event @types/jest @types/react-test-renderer jest jest-environment-jsdom react-test-renderer ts-jest ts-node
```

2.Create the file jest.setup.ts and paste the next code:
```ts
import "@testing-library/jest-dom/extend-expect";
```

3. Create the file jest.config.ts and paste the next code:

```ts
export default {
  testEnvironment: "jsdom",
  transform: {
    "^.+\\.tsx?$": ["ts-jest", { tsconfig: "tsconfig.json" }],
  },
  fakeTimers: { enableGlobally: true },
  setupFilesAfterEnv: ["<rootDir>/jest.setup.ts"],
};
```








