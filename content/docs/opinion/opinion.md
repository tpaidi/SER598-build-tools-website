---
weight: 9
title: "Analytical Component"
bookToc: true
bookFlatSection: false
---

# Analytical Component

### Main Opinions built from the below observations section and our experience using it ourselves.

# General Pros/Cons of the tools described

- Webpack is still the most followed and provides the most configuration. In the case that you plan to scale and create a much more configurable project for the long-run, it is better to go with webpack. Given its reach community support, it also feels like a safer option for new developers.

- Parcel seems to losing popularity mostly because it lacks in many areas like adoption of new features, community support, also seems to be a heavier bundler [even with Parcel 2](https://github.com/parcel-bundler/parcel/issues/4565) 

- Rollup and ESBuild on the other hand have been gaining popularity and have found their uses in different ways. Notably in the building of the popular framework Vite. Vite uses both Rollup and ESBuild. Reasons and resources have been attached but essentially, unlike Parcel, they are much closer to newer requirements of performance and optimizations.

# Opinions based on experience

- Rollup: 

  - Pro
    - Rollup was very straightforward to use and provided a lot of features to directly integrate. We enjoyed working with it and will consider it as a possible option as a build tool in the future.
  - Con
    - We did run into a few obscure issues related to code splitting and typescript. There definitely does seem to be some lack of flexibility in comparison to Webpack with some of these corner cases.

- Webpack:

  - Pro
    - Great documentation and support meant that we had a very smooth time building an application with webpack. Working with webpack was enjoyable and webpack will go forward as our first consideration for future developement.
  - Con
    - Lots of inbuilt configurations meant that it lacked a little simplicity. For smaller projects, it feels like webpack might be doing too much. 
---

### Overall Observations for the basis of general opinions above

We will go through and make observations regarding each of the main bundling tools.

# Webpack

- Webpack has good documentation and is very popular. We have two contrasting metrics below to show this. 
  - [Metric 1 - webpack](https://github.com/webpack/webpack) (64.8k stars at the time of writing this document)
  - [Metric 1 - esbuild](https://github.com/evanw/esbuild) (38.2k stars at the time of writing this)

  - [Metric 2](https://npmtrends.com/esbuild-vs-parcel-vs-rollup-vs-vite-vs-webpack). We have an npmtrends link here to show download statistics.

- An observation from the above

  - While it may seem like ESBuild has overtaken webpack, this is most likely because ESbuild has become a standard dependency to many other build tools, while Webpack still remains the dominant build tool.

- Code splitting seems to be an important feature webpack. This is used to improve load time. You basically split the code into multiple smaller bundles which can be loaded when needed. 
[Code Splitting](https://webpack.js.org/guides/code-splitting/)

- More preferable with projects that require a high amount of configurations and not for small projects. Generally held opinion and also mine. Obvious from the higher complexity of the configurations. 

- Really good support for plugins. 
[Plugins](https://webpack.js.org/plugins/) 

- Webpack naturally runs on a single threaded model. This is different from a build tool like parcel which has multicore processing built in. Third party tools like terserwebpack and thread-loader provide ways to handle parallel processing. 
[Terser-webpack](https://webpack.js.org/plugins/terser-webpack-plugin/) , [Thread-loader](https://webpack.js.org/loaders/thread-loader/)

# Parcel

- Little Configuration, automatic support for new file types and such. 
- Other automatic features include code splitting and hot module replacement.
- Seems to have automatic support for multicore processing. In contrast, a build tool like Webpack has to use third party tools as mentioned above.
[Parcel Source](https://parceljs.org/)

# Rollup

- Rollup is designed with performance in mind [Rollup documentation](https://rollupjs.org/). Here, we need to note that ESBuild is much faster than the like of Rollup, Webpack and even parcel. So when we say performance, it is not to say speed, but things like bundle size and bundle optimization and lots of plugins for optimizations. One example of the kind of optimization is tree-shaking 
[Tree Shaking](https://rollupjs.org/introduction/#tree-shaking)

- Simple with less configuration. Seems like the Rollup documentation provides a single command that gives us the option to add a configuration if needed. This form of automation does have some similarities to Parcel. [Rollup introduction](https://rollupjs.org/introduction/)

- Important point - Vite has adopted Rollup along with ESBuild. Vite uses Rollup as the main bundler. Rollup is still more flexible and as heavily contributed to the structure of Vite as it is today and it seems to be the case that in a performance vs flexibility trade-off, Vite has chosen flexibility. 
[Vite documentation](https://vitejs.dev/guide/why.html)

# ESBuild

- Fastest among all of the build tools seems to be ESBuild 
[Benchmark link 1](https://github.com/evanw/esbuild#benchmark-results). 
Independent source which has also given the testing criteria just like the github repo.
[Benchmark link 2](https://blog.logrocket.com/fast-javascript-bundling-with-esbuild/)

- Just like RollUp, there is minimal configuration required and hence reduce the learning curve. 
[Building the first bundle with esbuild](https://esbuild.github.io/getting-started/#your-first-bundle)

- Important point - Vite has adopted ESbuild along with Rollup. Vite pre-bundles dependencies using esbuild. [Vite documentation](https://vitejs.dev/guide/why.html)

---

