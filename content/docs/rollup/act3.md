---
title: "Activity 3"
---

# Rollup Demonstration: Activity 3 #

## Problem Overview ##
Activity 3 focuses on advanced optimizations and features that enhance modularity, maintainability, and flexibility in how bundles are created and distributed. The specific goals include:

1. **Code Splitting**: Divide the bundle into smaller chunks to improve performance by loading only the necessary code.
2. **Preserve Modules**: Keep the module structure intact during bundling, allowing for a more modular and maintainable codebase.
3. **Dynamic Imports**: Enable on-demand loading of code, useful for features or modules that aren't needed upfront.
4. **Multiple Output Formats**: Generate bundles in both CommonJS (`cjs`) and ES Module (`esm`) formats to support different environments.

These changes help optimize large projects, improve runtime performance, and ensure compatibility with various platforms.

---

## Activity 3: Code Splitting and Modular Bundling ##

### For this activity ###

The source code is available in the [GitHub Repository here](https://github.com/tpaidi/SER598-build-tools-tutorial/tree/main/rollup/rollupActivity3/).

### Folder Structure ###
```
activity3/
├── src/
│   ├── index.js
│   ├── another.js
│   ├── util.js
│   ├── helper.js
│   ├── dynamicImport1.js
│   ├── dynamicImport2.js
│   ├── styles.css
│   ├── index.html
├── package.json
├── rollup.config.js
├── dist/
│   ├── cjs/
│   │   ├── index.js
│   │   ├── another.js
│   │   ├── ...
│   ├── esm/
│   │   ├── index.js
│   │   ├── another.js
│   │   ├── ...
│   ├── bundle.css
│   ├── bundle-stats.html
```

---

### Steps to Implement Code Splitting, Modular Bundling, and Dynamic Imports ###

### Step 1: Code Splitting
Code splitting allows you to divide the application into smaller chunks, improving performance by loading only the necessary code.

#### Code Changes
Update `rollup.config.js` to include multiple entry points for splitting:
```javascript
input: ['./src/index.js', './src/another.js'],
```

#### Expected Outcome

- This step focuses on the outcome of doing code splitting as given above. Hence the below outcome is expected - 

  - The output bundles will contain separate files for `index.js`, `another.js`, and their dependencies, ensuring only relevant chunks are loaded.

![Code splitting gives us both modules in separate bundles](/docs/rollup/code_splitting_and_dynamic_import.png)
---

### Step 2: Preserve Modules
Preserving modules ensures that Rollup retains the original file structure during bundling. This makes it easier to debug and maintain the codebase.

#### Code Changes
In `rollup.config.js`, configure the `preserveModules` option for the CommonJS format:
```javascript
output: [
  {
    dir: 'dist/cjs',
    format: 'cjs',
    sourcemap: true,
    preserveModules: true, // Keep separate bundle files and module structure
  },
  {
    dir: 'dist/esm',
    format: 'esm',
    sourcemap: true,
  }
],
```

#### Expected Outcome

- This step is present for providing the difference in bundle creation when preserveModules option is used. Hence the following outcome is observed - 

  - The `dist/cjs/` folder will retain the modular structure of the source files.
  - The `dist/esm/` folder will contain bundled ES module files.

![Preserve modules option for output plugin prserving module structure](/docs/rollup/preserve_module.png)

---

### Step 3: Dynamic Imports
Dynamic imports enable on-demand loading of specific modules, improving performance by deferring the loading of rarely used features.

#### Code Changes
In `src/index.js`, use `import()` for dynamic imports:
```javascript
async function loadModule(moduleName) {

    if (moduleName === 'math') {
      const { add } = await import('./dynamicImport1.js');
      console.log(add(2, 3));
    } else if (moduleName === 'string') {
      const { toUpperCase } = await import('./dynamicImport2.js');
      console.log(toUpperCase('hello'));
    }

}

loadModule('math');
```

**Example Usage**
Dynamic imports allow `dynamicImport1.js` or `dynamicImport2.js` to be loaded only when needed:
```javascript
// src/dynamicImport1.js
function add(num1, num2) {
    return num1 + num2;
}

// src/dynamicImport2.js
function toUpperCase(stringToConvert) {
    return stringToConvert.toUpperCase();
}
```

#### Expected Outcome

- The above step is focused on preventing the loading of files that aren't required at a certain point of time. Hence the following should happen - 

  - The dynamically imported modules (`dynamicImport1.js` and `dynamicImport2.js`) will be loaded only when requested at runtime.

![dynamicImport bundle files](/docs/rollup/code_splitting_and_dynamic_import.png)
![Not used in bundle](/docs/rollup/dynamic_import.png)

---

### Step 4: Multiple Output Formats
Generating multiple output formats ensures compatibility with different environments, such as Node.js (using CommonJS) and browsers (using ES modules).

#### Code Changes
Configure multiple outputs in `rollup.config.js`:
```javascript
output: [
  {
    dir: 'dist/cjs',
    format: 'cjs',
    sourcemap: true,
    preserveModules: true,
  },
  {
    dir: 'dist/esm',
    format: 'esm',
    sourcemap: true,
  }
],
```

#### Expected Outcome

- The above step focuses on creating two separate output files for compatibility with different environment. Hence our output should have two dist folders and the expected output should look like - 

  - The `dist/cjs/` folder will contain CommonJS bundles.
  - The `dist/esm/` folder will contain ES module bundles.

![Multiple folders within dist](/docs/rollup/multiple_output_formats.png)

---

### Summary of Changes ###
1. **Code Splitting**: Divided the bundle into smaller chunks, reducing initial load time and improving performance.
2. **Preserve Modules**: Retained the module structure in the CommonJS output, improving maintainability and debugging.
3. **Dynamic Imports**: Implemented on-demand loading for specific modules, reducing the upfront bundle size.
4. **Multiple Output Formats**: Generated both CommonJS and ES module bundles to ensure compatibility with various platforms.
