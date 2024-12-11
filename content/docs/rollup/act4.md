---
title: "Activity 4"
---
# Rollup Demonstration: Activity 4 #

## Problem Overview ##
Activity 4 focuses on adding TypeScript support to the project. By converting all `.js` files to `.ts` and configuring Rollup to handle TypeScript, we gain the following benefits:

1. **Static Typing**: Improves code quality by catching errors at compile time.
2. **Better Tooling**: Provides better IDE support with autocompletion and type-checking.
3. **Cross-Environment Compatibility**: Generates type declaration files (`.d.ts`) for library consumers.

This activity introduces TypeScript-specific configurations, file conversions, and Rollup setup in a step-by-step manner.

---

## Activity 4: TypeScript Integration ##

### For this activity ###

The source code is available in the [GitHub Repository here](https://github.com/tpaidi/SER598-build-tools-tutorial/tree/main/rollup/rollupActivity4/).

### Folder Structure ###
```
activity4/
├── src/
│   ├── index.ts
│   ├── another.ts
│   ├── util.ts
│   ├── helper.ts
│   ├── dynamicImport1.ts
│   ├── dynamicImport2.ts
│   ├── styles.css
│   ├── index.html
├── tsconfig.json
├── package.json
├── rollup.config.js
├── dist/
│   ├── cjs/
│   │   ├── index.js
│   │   ├── another.js
│   │   ├── types/
│   │       ├── index.d.ts
│   │       ├── another.d.ts
│   ├── esm/
│   │   ├── index.js
│   │   ├── another.js
│   │   ├── types/
│   │       ├── index.d.ts
│   │       ├── another.d.ts
│   ├── bundle.css
│   ├── bundle-stats.html
```

---

### Steps to Implement TypeScript Support ###

### Step 1: Install the TypeScript Plugin
To enable TypeScript support, we need the Rollup TypeScript plugin and TypeScript itself.

#### Installation
Run the following command to install the required packages:
```bash
npm install --save-dev @rollup/plugin-typescript typescript
```

---

### Step 2: Add TypeScript Configuration
A `tsconfig.json` file is needed to specify TypeScript compiler options. This ensures consistent behavior across the project.

#### Code
Create `tsconfig.json` with the following content:
```json
{
    "compilerOptions": {
      "target": "ESNext",
      "module": "ESNext",
      "declaration": true,
      "strict": true,
      "esModuleInterop": true,
      "skipLibCheck": true,
      "forceConsistentCasingInFileNames": true,
      "moduleResolution": "node"
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "dist"]
}
```

#### Key Options
- `declaration`: Generates type declaration files (`.d.ts`).
- `strict`: Enables all strict type-checking options.
- `esModuleInterop`: Ensures compatibility with CommonJS modules.
- `moduleResolution`: Specifies how modules are resolved (`node`).

---

### Step 3: Convert JavaScript to TypeScript
Rename all `.js` files in the `src/` folder to `.ts` and add type annotations where applicable.

#### Example Conversion
**Before (JavaScript):**
```javascript
export function add(a, b) {
    return a + b;
}
```

**After (TypeScript):**
```typescript
export function add(a: number, b: number): number {
    return a + b;
}
```

Repeat this process for all files in the `src/` folder.

---

### Step 4: Update Rollup Configuration
The Rollup configuration needs to include the TypeScript plugin and use the `tsconfig.json` file for settings.

#### Code
Add the TypeScript plugin to `rollup.config.js`:
```javascript
import typescript from '@rollup/plugin-typescript';

const plugins = [
  // Other plugins
  typescript({
    tsconfig: './tsconfig.json',
    declaration: true,
    declarationDir: 'dist/types',
  }),
];
```

Configure separate outputs for CommonJS and ES Modules:
```javascript
export default [
  {
    input: ['./src/index.ts', './src/another.ts'],
    external: ['lodash'],
    output: {
      dir: 'dist/cjs',
      format: 'cjs',
      sourcemap: true,
      preserveModules: true,
    },
    plugins: plugins,
  },
  {
    input: ['./src/index.ts', './src/another.ts'],
    external: ['lodash'],
    output: {
      dir: 'dist/esm',
      format: 'esm',
      sourcemap: true,
    },
    plugins: plugins,
  },
];
```

#### Key Changes
- **`declaration`**: Enables generation of `.d.ts` files.
- **`declarationDir`**: Specifies the output directory for type declaration files.

---

### Step 5: Generate Type Declarations
Run the Rollup build command to generate the output bundles and type declaration files.

#### Command
```bash
npm run build
```

#### Expected Output

- The above changes are focused on taking are typescript application and providing the similar results as we would get for a normal js application. For the same, we would expect the following output - 

  - Bundled JavaScript files in `dist/cjs` and `dist/esm`.
  - Type declaration files (`.d.ts`) in `dist/cjs/types` and `dist/esm/types`.

---

### Final Output Screenshot

- [Final typescript Bundle after following the above steps](/docs/rollup/typescript_bundle.png)

---

### Summary of Changes ###
1. **Install TypeScript Support**: Added the Rollup TypeScript plugin and TypeScript itself.
2. **TypeScript Configuration**: Created `tsconfig.json` to define compiler options.
3. **File Conversion**: Renamed all `.js` files to `.ts` and added type annotations.
4. **Type Declaration Files**: Configured Rollup to generate `.d.ts` files for both CommonJS and ES module outputs.
