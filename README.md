# **Webpack Template Repository**

This repository is designed to be used as a **template for Webpack projects**. It includes a minimal Webpack setup with **webpack-merge** for managing development and production configurations. To customize the setup, refer to the guide below on how to add **CSS, image, and HTML loaders** as needed for your project.


## **1. Install Required Loaders**

### **CSS Loader**

To process CSS files, install the necessary loaders:

```bash
npm install --save-dev style-loader css-loader
```

Then, modify `webpack.common.js` to include the CSS rule:

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

### **Image Loader**

To handle images like PNG, JPG, SVG, and GIF files, install:

```bash
npm install --save-dev file-loader
```

Then, modify `webpack.common.js` to include the image rule:

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif|svg)$/i,
        type: "asset/resource",
      },
    ],
  },
};
```

### **HTML Loader**

To process HTML files and automatically inject bundled JavaScript, install:

```bash
npm install --save-dev html-webpack-plugin
```

Then, modify `webpack.common.js` to use the plugin:

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
  ],
};
```

## **2. Updating Project Structure**

Ensure your project has an `index.html` file inside the `src/` directory:

```
my-project/
├── src/
│   ├── index.js
│   ├── index.html
│   ├── styles.css (if using CSS)
│   ├── assets/
│   │   ├── image.png (if using images)
├── dist/
├── package.json
├── webpack.common.js
├── webpack.dev.js
├── webpack.prod.js
```

## **3. Configure Webpack with `webpack-merge`**

### **Install webpack-merge**

```bash
npm install --save-dev webpack-merge
```

### **Common Configuration (`webpack.common.js`)**

```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
  ],
};
```

### **Development Configuration (`webpack.dev.js`)**

```javascript
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "development",
  devServer: {
    static: "./dist",
    hot: true,
    open: true,
  },
});
```

### **Production Configuration (`webpack.prod.js`)**

```javascript
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "production",
});
```

## **4. Available npm Scripts**

Modify your `package.json` file to include these scripts:

```json
"scripts": {
  "start": "webpack serve --open --config webpack.dev.js",
  "build": "webpack --config webpack.prod.js",
  "lint": "eslint src --ext .js",
  "format": "prettier --write src"
}
```

### **Script Descriptions**

- **`start`** → Starts the Webpack development server with live reloading using the development config.
- **`build`** → Creates a production-ready bundle using the production config.
- **`lint`** → Runs ESLint to check for JavaScript syntax errors and warnings.
- **`format`** → Formats all JavaScript files using Prettier.

## **5. Running Webpack**

After installing the loaders, restart your development server:

```bash
npm run start
```

Or create a new production build:

```bash
npm run build
```

Your project is now configured to handle **CSS, images, and HTML** using Webpack, along with essential scripts for development and code quality, while leveraging **webpack-merge** for a modular configuration.

