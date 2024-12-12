---
title: "Activity 1"
---
# Rollup Demonstration: Activity 1 #

## Problem Overview ##
Modern web applications consist of multiple JavaScript files, stylesheets, and HTML templates. Managing these files manually introduces several challenges:

1. **Dependency Order Issues**:
   - Developers must ensure scripts are loaded in the correct order. Errors can arise when dependent files are loaded before their dependencies.

2. **Global Namespace Pollution**:
   - Without tools, code sharing between files relies on global variables, leading to potential conflicts as projects grow.

3. **Performance Bottlenecks**:
   - Each JavaScript file and stylesheet requires a separate HTTP request, which slows down page load times.

4. **Code Optimization and Minification**:
   - Manually minifying code and optimizing assets for production is tedious and error-prone.

5. **Modular Code Reuse**:
   - Reusing modular code across projects without a bundler like Rollup is difficult and limits scalability.

Rollup solves these issues by bundling modules, handling CSS files, generating optimized builds, and performing advanced optimizations like tree shaking (removing unused code). This makes Rollup particularly effective for both application and library development, resulting in smaller, faster, and more maintainable projects.

---

## Activity 1: Without Rollup ##

### For this activity ###

The source code is available in the [GitHub Repository here](https://github.com/tpaidi/SER598-build-tools-tutorial/tree/main/rollup/rollupActivity1/).

### Folder Structure ###
```
activityNonRollup/
├── index.html
├── index.js
├── math.js
```

### Code ###

**math.js**
```javascript
function add(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

window.add = add;
window.multiply = multiply;
```

**index.js**
```javascript
console.log("Sum:", add(2, 3));  
console.log("Product:", multiply(2, 3)); 
```

**index.html**
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Without Rollup</title>
        <!-- Load scripts manually in the incorrect order -->
        <script src="index.js"></script>
        <script src="math.js"></script>
    </head>
    <body>
        <h1>Without Rollup</h1>
    </body>
</html>
```

**Error Screenshot**

![Error output](/docs/rollup/error.png)

**Correct Alternative**

**index.html**
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Without Rollup</title>
        <!-- Load scripts manually in the correct order -->
        <script src="math.js"></script>
        <script src="index.js"></script>
    </head>
    <body>
        <h1>Without Rollup</h1>
    </body>
</html>
```

**Correct Screenshot**

![Correct output](/docs/rollup/works.png)

**Why It Fails**
- Manual Dependency Management:  
  If the `<script>` tags in index.html are not in the correct order (for example, loading `index.js` before `math.js`), the browser will throw an error like:  
  `Uncaught ReferenceError: add is not defined`

- Global Namespace Pollution:  
  All functions are attached to the global `window` object, increasing the risk of name conflicts as the application grows.

- Performance Issues:  
  Each `<script>` tag results in a separate HTTP request, slowing down page load times.

## Activity 1: With Rollup ##

### Folder Structure ###
```
activity1rollup/
├── src/
│   ├── index.js
│   ├── math.js
│   ├── index.html
├── package.json
├── rollup.config.js
├── dist/
│   ├── bundle.js
│   ├── bundle.css
│   ├── index.html
```

### Code ###

**src/math.js**
```javascript
export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}
```

**src/index.js**
```javascript
import { add, multiply } from './math.js';

console.log("Sum:", add(2, 3));
console.log("Product:", multiply(2, 3));
```

**src/index.html**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rollup Project</title>
  <link rel="stylesheet" href="bundle.css">
</head>
<body>
  <h1>Hello, Rollup!</h1>
  <script src="bundle.js"></script>
</body>
</html>
```

---

## What Is rollup.config.js? ##
The `rollup.config.js` file is the configuration file for Rollup. It defines how Rollup processes and bundles files. You will need to add this file directly to the directory at the root level.

**rollup.config.js**
```javascript
import html from '@rollup/plugin-html';
import css from 'rollup-plugin-css-only';
import path from 'path';
import fs from 'fs';

export default {
  input: './src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'esm',
    sourcemap: true,
  },
  plugins: [
    html({
      fileName: 'index.html',
      template: () => {
        const htmlPath = path.resolve(__dirname, 'src/index.html');
        return fs.readFileSync(htmlPath, 'utf8');
      },
    }),
    css({ output: 'bundle.css' }),
  ],
};
```

**Important Parts of rollup.config.js**
- **input**: Specifies the entry point of the application (`src/index.js`).
- **output**: Defines the output file (`dist/bundle.js`), the format (ES modules), and enables source maps for debugging.
- **plugins**:  
  - `@rollup/plugin-html`: Generates the HTML file for the project.  
  - `rollup-plugin-css-only`: Extracts CSS into `bundle.css`.

---

## Steps to Set Up Rollup ##

1. Initialize the Project (if needed):  
   `npm init -y`

2. Install Rollup and Plugins:  
   ```bash
   npm install --save-dev rollup rollup-plugin-html 
   npm install --save-dev rollup-plugin-css-only
   npm install --save-dev http-server
   npm install --save-dev @rollup/plugin-html
   ```

3. Add the rollup.config.js file as given in the earlier section.

4. Create or Copy Required Files:  
   Create `index.js`, `index.html`, and `style.css` in the `src/` folder. For the purpose of this tutorial, we have pre-built files in the GitHub repository.

5. Add Scripts to package.json:  
   For example:  
   ```json
   "scripts": {
       "build": "rollup -c",
       "serve": "http-server dist"
   }
   ```

6. Run Rollup:  
   - `npm run build` to generate the `dist` folder with `bundle.js`, `bundle.css`, and `index.html`.
   - `npm run serve` to start a local server and view your application in the browser.

---

## Why Rollup Helps ##
1. Dependency Resolution:  
   Rollup supports ES6 module syntax and automatically resolves imports and exports, ensuring that dependencies are included in the correct order.

2. Tree Shaking:  
   Rollup eliminates unused code, resulting in smaller and more efficient bundles. This is particularly beneficial for large projects and libraries.

3. Plugin Ecosystem:  
   A rich ecosystem of plugins enables handling various tasks—such as bundling CSS, generating HTML, and minifying code—without manual intervention.

4. Code Optimization:  
   Rollup produces highly optimized bundles by default. Combined with tree shaking and minification plugins, this leads to faster load times.

5. Modular Code Structure:  
   Encourages writing modular, maintainable code. Developers can break down the codebase into logical modules, improving scalability.

6. Format Flexibility:  
   Rollup can output multiple formats (ES modules, CommonJS, etc.) to ensure compatibility with different browsers and environments.

---

## Conclusion ##

Without Rollup:
- Developers must manually manage script and CSS loading.
- Incorrect import orders lead to runtime errors.
- Multiple HTTP requests slow down page load times.

With Rollup:
- Dependencies are resolved automatically.
- Tree shaking eliminates unused code, reducing bundle size.
- Script and CSS order are handled by Rollup, simplifying project management.
- The project becomes more scalable, maintainable, and optimized for both development and production environments.
