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