---
bookToc: true
title: "Activity 2"
---
# Activity 2- Demonstrate the full range of WebPack's functionalities
# Cosmic Explorer

## Overview

### For This Activity

Now that we have seen some basic benefits and functionalitites that Webpack provides, in this activity we will build a more complex website than in Activity 1, demonstrating the full use of webpack by working with a project that includes multiple CSS files, static assets, JavaScript, and HTML files, with the source code available in the [GitHub Repository here](https://github.com/tpaidi/SER598-build-tools-tutorial/tree/main/webpack/activity2/cosmic-explorer).

### Brief of the Website

**Cosmic Explorer** features an interactive gallery of celestial objects, including planets, stars, and galaxies. Each object has detailed information and dynamically loaded facts available upon user interaction.

### Multiple Folders Structure

The project structure consists of various folders that separate concerns such as styles, scripts, utilities, images, and fonts. This modular approach keeps the codebase organized but also highlights how complex projects can become without a build tool like webpack.

### How to Clone and Run It

1. Clone the repository:
   ```bash
   git clone https://github.com/tpaidi/SER598-build-tools-tutorial.git
   ```
2. Navigate to the project directory:
   ```bash
   cd webpack/activity2/cosmic-explorer
   ```
3. Open `index.html`

On opening `index.html` you will be met with a homepage like this of our beautiful application! 

![Cosmic Explorer Home Page](/docs/webpack/webpackact2.png "Cosmic Explorer Home Page")

Feel free to explore around and play with the website before we discuss why its so inefficient ;)

## Current Inefficiencies Without Webpack

- **Manual File Management:** All JavaScript and CSS files must be manually referenced in HTML, making updates tedious.

- **No Automatic Optimization:** There is no minification, compression, or bundling, resulting in larger file sizes and slower load times.

- **Multiple HTTP Requests:** Every JavaScript, CSS, and image file is loaded separately, causing additional server requests and slower performance.

- **No Tree Shaking:** Unused functions remain in the code (e.g. `unusedFunction()` inside the `utils` folder), inflating the bundle size.

- **No Code Splitting or Lazy Loading:** Features like dynamic loading are managed manually, increasing maintenance complexity.

- **No Asset Management:** There is no automatic copying or processing of static assets, requiring developers to manage paths and updates manually.

By addressing these limitations with webpack, we can optimize and streamline the development process, reduce manual intervention, and improve site performance.

Now lets implement Webpack in our application to improve it and take advantage of all the benefits it provides!

## 1. Setting Up Webpack in our project

We can start by copying our Part 1 of the activity into a fresh directory and go from there

1. **Initialize the Project:**
   ```bash
   npm init -y
   npm install --save-dev webpack webpack-cli webpack-dev-server
   ```

2. **Add Loaders and Plugins:**
   ```bash
   npm install --save-dev css-loader style-loader mini-css-extract-plugin html-webpack-plugin
   ```

3. **Create `webpack.config.js`:**
   Define how to process files, where to output them, and how to generate HTML:
   ```javascript
	const path = require('path');
	const HtmlWebpackPlugin = require('html-webpack-plugin');
	const MiniCssExtractPlugin = require('mini-css-extract-plugin');
	module.exports = {
	entry: {
		main: './src/index.js',
		planet: './src/planet.js'
	},
	output: {
		filename: '[name].[contenthash].js',
		path: path.resolve(__dirname, 'dist'),
		clean: true
	},
	mode: 'development',
	devServer: {
		static: './dist',
		port: 3000,
		hot: true
	},
	module: {
		rules: [
		{
			test: /\.css$/i,
			use: [MiniCssExtractPlugin.loader, 'css-loader']
		},
		{
			test: /\.(png|jpe?g|svg|gif)$/i,
			type: 'asset/resource'
		},
		{
			test: /\.(woff|woff2|ttf|eot|otf)$/i,
			type: 'asset/resource'
		}
		]
	},
	plugins: [
		new HtmlWebpackPlugin({
		template: './src/templates/index.html',
		filename: 'index.html',
		chunks: ['main']
		}),
		new HtmlWebpackPlugin({
		template: './src/templates/planet.html',
		filename: 'planet.html',
		chunks: ['planet']
		}),
		new MiniCssExtractPlugin({
		filename: '[name].[contenthash].css'
		})
	],
	optimization: {
		splitChunks: {
		chunks: 'all'
		}
	}
	};
   ```
#### Explanation of Key Sections:

1. **Entry Points:**
   ```javascript
   entry: {
     main: './src/index.js',
     planet: './src/planet.js'
   },
   ```
   - Specifies multiple entry points: `main` for the main page (`index.html`) and `planet` for the planet details page (`planet.html`).
   - Each entry point generates its own JavaScript bundle.

2. **Output Configuration:**
   ```javascript
   output: {
     filename: '[name].[contenthash].js',
     path: path.resolve(__dirname, 'dist'),
     clean: true
   },
   ```
   - `[name]` dynamically names the output file based on the entry point name (e.g., `main.[hash].js`, `planet.[hash].js`).
   - `contenthash` ensures that filenames change when the file content changes, enabling efficient caching.
   - `path` specifies the directory where the bundled files will be output (`dist`).
   - `clean: true` ensures the `dist` folder is cleaned before each build.

3. **Mode:**
   ```javascript
   mode: 'development',
   ```
   - Specifies the build mode (`development` or `production`). In `development`, Webpack prioritizes speed and readability over optimization.

4. **DevServer Configuration:**
   ```javascript
   devServer: {
     static: './dist',
     port: 3000,
     hot: true
   },
   ```
   - Serves the application locally from the `dist` directory.
   - `hot: true` enables Hot Module Replacement (HMR) for live-reloading during development.

5. **Module Rules:**
   ```javascript
   module: {
     rules: [
       {
         test: /\.css$/i,
         use: [MiniCssExtractPlugin.loader, 'css-loader']
       },
       {
         test: /\.(png|jpe?g|svg|gif)$/i,
         type: 'asset/resource'
       },
       {
         test: /\.(woff|woff2|ttf|eot|otf)$/i,
         type: 'asset/resource'
       }
     ]
   },
   ```
   - **CSS Rule:** Processes `.css` files using `css-loader` (to bundle CSS) and `MiniCssExtractPlugin.loader` (to extract CSS into separate files).
   - **Image Rule:** Handles image files (e.g., `.png`, `.jpg`, `.svg`) as `asset/resource`, moving them to the output directory and replacing file references with the correct paths.
   - **Font Rule:** Similar to images, processes font files and outputs them to the `dist` folder.

6. **Plugins:**
   ```javascript
   plugins: [
     new HtmlWebpackPlugin({
       template: './src/templates/index.html',
       filename: 'index.html',
       chunks: ['main']
     }),
     new HtmlWebpackPlugin({
       template: './src/templates/planet.html',
       filename: 'planet.html',
       chunks: ['planet']
     }),
     new MiniCssExtractPlugin({
       filename: '[name].[contenthash].css'
     })
   ],
   ```
   - **HtmlWebpackPlugin:** Automatically generates `index.html` and `planet.html` with the correct `<script>` tags for their respective bundles.
     - `chunks` ensures only the relevant bundle (`main` or `planet`) is included in each page.
   - **MiniCssExtractPlugin:** Extracts CSS into separate files for better caching and smaller JS bundles.

7. **Optimization:**
   ```javascript
   optimization: {
     splitChunks: {
       chunks: 'all'
     }
   }
   ```
   - Enables code splitting for shared dependencies (e.g., third-party libraries like `lodash`) to avoid duplicating code across bundles.

---

This configuration allows for multiple entry points, optimized output, automated asset handling, and dynamic HTML generation, providing a scalable and maintainable build system for the Cosmic Explorer project.



## 2. Structuring Your Code for Webpack

### Move Files into the `src` Folder

Restructure your project so that all source files (JavaScript, CSS, HTML, images) are inside the `src` folder. Your updated directory structure should look like this:

```
cosmic-explorer-webpack/
├── src/
│   ├── templates/
│   │   ├── index.html
│   │   ├── planet.html
│   ├── facts.js
│   ├── gallery.js
│   ├── index.js
│   ├── planet.js
├── fonts/
│   ├── cosmic-font.ttf
├── utils/
│   ├── format.js
├── styles/
│   ├── main.css
│   ├── gallery.css
│   ├── planet.css
├── images/
│   ├── earth.jpg
│   ├── mars.jpg
│   ├── jupiter.jpg
│   ├── sirius.jpg
│   ├── betelgeuse.jpg
│   ├── andromeda.jpg
│   ├── milkyway.jpg
├── webpack.config.js
├── package.json
```

### Import Assets in JavaScript

- In `index.js`, instead of linking CSS in HTML, `import` it, add these lines to the top of your `index.js`:
  ```javascript
  import '../styles/main.css';
  import '../styles/gallery.css';
  import { initGallery } from './gallery';
  ```

- In `planet.js` import both the CSS files, add these lines to the top of your `planet.js` file:
  ```javascript
  import '../styles/main.css';
  import '../styles/gallery.css';
  ```

- In `gallery.js`, `import` images, now use these references within the file instead of paths, also export our `initGallery()` function

Final code of `gallery.js` looks like this:
  ```javascript
	import earth from '../images/earth.jpg';
	import mars from '../images/mars.jpg';
	import jupiter from '../images/jupiter.jpg';
	import sirius from '../images/sirius.jpg';
	import betelgeuse from '../images/betelgeuse.jpg';
	import andromeda from '../images/andromeda.jpg';
	import milkyway from '../images/milkyway.jpg';

	const cosmicObjects = [
		{ type: 'planet', name: 'Earth', img: earth, desc: 'The blue planet, teeming with life.', slug: 'earth' },
		{ type: 'planet', name: 'Mars', img: mars, desc: 'The red planet, with hints of past water.', slug: 'mars' },
		{ type: 'planet', name: 'Jupiter', img: jupiter, desc: 'A gas giant with a famous Great Red Spot.', slug: 'jupiter' },
		{ type: 'star', name: 'Sirius', img: sirius, desc: 'The brightest star in Earth’s night sky.', slug: 'sirius' },
		{ type: 'star', name: 'Betelgeuse', img: betelgeuse, desc: 'A red supergiant star nearing the end of its life.', slug: 'betelgeuse' },
		{ type: 'galaxy', name: 'Andromeda', img: andromeda, desc: 'A spiral galaxy on a collision course with the Milky Way.', slug: 'andromeda' },
		{ type: 'galaxy', name: 'Milky Way', img: milkyway, desc: 'Our home galaxy, containing billions of stars.', slug: 'milkyway' },
	];
	
	export function initGallery() {
		const galleryDiv = document.getElementById('gallery');
		galleryDiv.innerHTML = '';
		
		cosmicObjects.forEach(obj => {
		const itemDiv = document.createElement('div');
		itemDiv.className = 'gallery-item';
		itemDiv.innerHTML = `
			<img src="${obj.img}" alt="${obj.name}">
			<h2>${obj.name}</h2>
			<p>${obj.desc}</p>
			<a href="planet.html?obj=${obj.slug}">View Details</a>
		`;
		galleryDiv.appendChild(itemDiv);
		});
	}
  ```

- Do the same thing in `planet.js`, no code this time because take it as a self exercise!

<sub><sup> Just kidding, final code is present in the repository if you need it! :)</sup></sub>


- Export the `showFact()` function in `fact.js`
```javascript
	export function showFact() {
```

**Why does this work?** Because now Webpack knows to process these files, bundle them, and optimize them. No more manual `<link>` or `<script>` tags!
Since we import the images of modules, Webpack now keeps track of the locations and updates the references in the bundle output, This ensures path is always correct

## 3. Dynamic Imports and Code Splitting

To load “interesting facts” on-demand for the planet detail page:

- In `planet.js`, replace `getElementById('loadFactsBtn').addEventListener` with the below code:
  ```javascript
  document.getElementById('loadFactsBtn').addEventListener('click', async () => {
    const { showFact } = await import('./facts'); // dynamically imports `facts.js`
    showFact(slug);
  });
  ```

This tells webpack to split `facts.js` into a separate chunk, only fetched when the user clicks the button, improving initial load times.

## 4. Tree Shaking and Production Builds

- **Tree Shaking:**
  Export functions you need, and ensure unused code isn’t imported anywhere. In production mode:
  ```bash
  npx webpack --mode production
  ```
  Webpack removes unused code automatically.

- **Check for Unused Code:**
  For example, if `unusedFunction()` in `format.js` is never imported, it won’t appear in the production bundle.

## 5. Running and Testing

- **Development Server:**
  ```bash
  npx webpack serve
  ```
  This launches a dev server with live reloading at `http://localhost:3000`.

- **Production Build:**
  ```bash
  npx webpack --mode production
  ```
  This generates an optimized, minified build in `dist/`.

## 6. Verifying the Results

Open `http://localhost:3000` and see the gallery. Click “View Details” to navigate to `planet.html`. The CSS, images, and JavaScript are automatically injected and optimized by webpack.

- Check that the “Show Interesting Facts” button dynamically loads `facts.js`.
- Inspect `dist/` to confirm the presence of hashed filenames and reduced file sizes.
![Dist](/docs/webpack/dist.png "dist")

## 7. Troubleshooting

If something doesn’t work:

- Ensure correct relative paths when importing CSS and images.
- Make sure `planet.js` and `facts.js` are properly included via the `HtmlWebpackPlugin` configuration and/or dynamic imports.
- Use production mode to ensure tree shaking and minification are applied.

---
![Alt text](<Screenshot 2024-12-11 at 11.31.14 PM.png>)
## Conclusion

By integrating webpack:

- We eliminated manual script and style references.
- Enabled code splitting and lazy loading for improved performance.
Below we can see how on only clicking on any planet, it's corresponding JavaScript packet gets called, this shows us the benefit of code splitting

![Lazy](/docs/webpack/lazy.png "lazy")

- Introduced tree shaking to remove unused code, reducing bundle sizes.

1. Here we can see code we have used below being generated when the final dist bundle is created, hence it's present twice, once in the development code and once in the dist code
![used](/docs/webpack/used.png "used")

2. In contrast, below we see some code which was not being used in the app not bundled into the dist code. 
![unused](/docs/webpack/unused.png "unused")

This shows us tree shaking and its benefits in action!

- Automated HTML generation, making it easier to scale and maintain the app.


This tutorial shows how a manual setup can evolve into a modern, optimized build system that improves maintainability, scalability, and performance.


Completed code after this activity can be found on the [GitHub Repository here](https://github.com/tpaidi/SER598-build-tools-tutorial/tree/main/webpack/activity2webpack/cosmic-explorer-webpack).