---
bookToc: false
title: "Activity 1"
---
# **Webpack Demonstration: Activities 1 and 2**

## **Problem Overview**
Modern web applications often consist of multiple JavaScript files. Managing these files manually can lead to several challenges, such as:

1. **Dependency Order Issues**:
   - Without proper tools, developers must manually ensure that scripts are loaded in the correct order.
   - Incorrect order causes runtime errors.

2. **Global Namespace Pollution**:
   - Sharing code between files relies on global variables, increasing the risk of variable name conflicts.

3. **Performance Limitations**:
   - Each JavaScript file generates an HTTP request, increasing page load times.

4. **Scalability Issues**:
   - Managing dependencies manually becomes unmanageable as the application grows.

Webpack is a tool that solves these problems by bundling JavaScript files and resolving dependencies automatically. This tutorial demonstrates these concepts step-by-step through two activities.

---

## **Activity 1: Without Webpack**

### **Folder Structure**
```
activity1/
├── index.html
├── index.js
├── math.js
```

### **Code**

#### `math.js`
```javascript
// math.js
function add(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

// Attach functions to the global window object
window.add = add;
window.multiply = multiply;
```

#### `index.js`
```javascript
// index.js
console.log("Sum:", add(2, 3)); // Access global add function
console.log("Product:", multiply(2, 3)); // Access global multiply function
```

#### `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Without Webpack</title>
        <!-- Load scripts manually in the correct order -->
        <script src="math.js"></script>
        <script src="index.js"></script>
    </head>
    <body>
        <h1>Without Webpack</h1>
    </body>
</html>
```

### **Why It Fails**
1. **Manual Dependency Management**:
   - If the `<script>` tags in `index.html` are not in the correct order (e.g., `index.js` before `math.js`), the browser will throw an error:
     ```
     Uncaught ReferenceError: add is not defined
     ```

2. **Global Namespace Pollution**:
   - All functions are attached to the global `window` object, increasing the risk of name conflicts as the application grows.

3. **Performance Issues**:
   - Each `<script>` tag results in a separate HTTP request, slowing down page load times.

---

## **Activity 2: With Webpack**

### **Folder Structure**
```
activity1webpack/
├── src/
│   ├── index.js
│   ├── math.js
├── index.html
├── package.json
├── webpack.config.js
├── dist/
│   ├── (generated bundle.js)
```

### **Code**

#### `src/math.js`
```javascript
// math.js
export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}
```

#### `src/index.js`
```javascript
// index.js
import { add, multiply } from './math.js';

console.log("Sum:", add(2, 3));
console.log("Product:", multiply(2, 3));
```

#### `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <title>With Webpack</title>
        <script src="dist/bundle.js"></script>
    </head>
    <body>
        <h1>With Webpack</h1>
    </body>
</html>
```

### **What Is `webpack.config.js`?**
The `webpack.config.js` file is the configuration file for Webpack. It defines how Webpack processes and bundles your files.

#### **Code**
```javascript
const path = require('path');

module.exports = {
    entry: './src/index.js', // Entry file
    output: {
        filename: 'bundle.js', // Output bundle file
        path: path.resolve(__dirname, 'dist'), // Output directory
    },
    mode: 'development', // Set mode to development for debugging
};
```

### **Important Parts of `webpack.config.js`**
1. **`entry`**:
   - Specifies the entry point of the application (`src/index.js`).

2. **`output`**:
   - Defines the output file (`dist/bundle.js`) and directory (`dist/`).

3. **`mode`**:
   - Defines the build mode (`development` or `production`).

---

### **Steps to Set Up Webpack**

1. **Initialize the Project**:
   Run the following command to create a `package.json` file:
   ```bash
   npm init -y
   ```

2. **Install Webpack**:
   Install Webpack and Webpack CLI as development dependencies:
   ```bash
   npm install webpack webpack-cli --save-dev
   ```

3. **Add Build Script**:
   Update `package.json` to include a build script:
   ```json
   "scripts": {
       "build": "webpack"
   }
   ```

4. **Run Webpack**:
   Generate the bundle file by running:
   ```bash
   npm run build
   ```

   This command creates the `dist/bundle.js` file.

5. **Open in Browser**:
   Open `index.html` in your browser. Ensure that it loads `dist/bundle.js`.

---

### **Why Webpack Succeeds**
1. **Dependency Resolution**:
   - Webpack automatically resolves dependencies and bundles all files into `dist/bundle.js`.

2. **Script Order**:
   - Script order in `index.html` doesn’t matter because Webpack handles dependencies internally.

3. **Performance**:
   - Only one HTTP request is made to load `bundle.js`, improving page load times.

4. **Scalability**:
   - Webpack simplifies managing large projects by modularizing code and resolving dependencies.

---

## **Comparison Table**
| Feature                  | Without Webpack                         | With Webpack                       |
|--------------------------|------------------------------------------|-------------------------------------|
| **Dependency Management** | Manual imports, error-prone             | Automatic resolution               |
| **Bundling**              | Not supported                           | Combines files into one bundle     |
| **Browser Compatibility** | Only modern browsers                    | Supports older browsers if configured |
| **HTTP Requests**         | Multiple requests for each file         | Single request for the bundle      |
| **Code Splitting**        | Difficult to implement manually         | Supported out of the box           |

---

## **Conclusion**
1. **Without Webpack**:
   - Developers must manage script loading manually.
   - Incorrect import orders cause runtime errors.
   - Multiple HTTP requests increase load times.

2. **With Webpack**:
   - Dependencies are resolved automatically.
   - Script order doesn’t matter.
   - The project is more scalable, maintainable, and optimized.