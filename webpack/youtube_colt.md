# Webpack

Webpack bundle all different files in a single structure inside a dist folder with js, css, and html files

`npm install --save-dev webpack webpack-cli`

Folder structure

```properties
root
|__ dist
|__ src
|    |__ index.js
|__ index.html
```

By default without a webpack config file webpack will :

1. Create dist folder at <rootDir>/dist index.js bundled <rootDir>/dist/index.js
2. default entry point is at <rootDir>/src/index.js
3. mode option is not provided then webpack will fallback to production mode.
4. as webpack fallback to production mode the dist/index.js is minified.
5. Inside <rootDir>/index.html a script tag should point to src="/dist/index.js"

## Webpack config

### 1. entry

```js
module.exports = {
	entry: './src/index.js',
};
```

Default path but another path can be confirgured

### 2. output

```js
const path = require('path');
module.exports = {
	output: {
		filename: 'index.js',
		path: path.resolve(__dirname, dist),
	},
};
```

path.resolve will retrieve the absolute path with the folder name where output filname should be stored.

### 3. mode

```js
    module.exports ={
        mode: "development
    }
```

mode:"development" will bundle <rootDir>/src/index.js in <rootDir>/dist/index.js unminified

### 4. devtool

```js
module.exports = {
	devtool: false,
};
```

devtool: false, will remove the eval function and show the code from <rootDir>/src/index.js unchanged.
this option helps to see our code in the <roorDir>/dist/index.js

### 5. module ( Loader )

css-loader and style-loader :

```properties
    npm install --save-dev css-loader style-loader
```

```js
    module.exports = {
        module: {
            rules : [
                test: /\.css$/,
                use: [  "style-loader",     // 2. inject style into dom ( header in index.html )
                        "css-loader"        // 1. turn css into commonjs
                ]
            ]
        }
    }
```

css-loader will check for all the css file imported in js and load/transform them into javascript inside <rootDir>/dist/index.js
at this stage our app is not showing any styling. We need style-loader to create a style tag with all css and automatically in <rootDir>/index.html header tag.

html-loader :

```properties
    npm install --save-dev html-loader
```

```js
    module.exports = {
        {
            test: /\.html$/,
            use: ["html-loader"],
        },
        {
            test: /\.(svg|png|jpg|gif)$/,
            use: {
                loader: "file-loader",
                options: {
                    name: "[name].[hash].[ext]
                }
        }

    }
```

html-loader will check in the `<roorDir> /src/index.html` every img tag src and will require it in js
`<img src="./webpack.png" />`
But we need to add file-loader so webpack can load the file

file-loader: allow to import all the assets like images (svg, png, jpg, ...)
by checking the src attr of img tag

```json
{
    test: /\.(svg|png|jpg)$/,
    use: {
        loader: "file-loader",
        options: {
            name: "[name].[hash].[ext]",
            outputPath: "img",
        },
    },
},
```

#### 6. PLUGIN

```properties
    npm install --save-dev html-webpack-plugin
```

HtmlWebpackplugin will automatically update the srcipt with the new hash in the filename.
Also if not specified, it will create the <rootDir>/dist/index.html from <rootDir>/index.html

```js
const HtmlWebpackplugin = require('html-webpack-plugin');
module.exports = {
	plugins: [
		new HtmlWebpackplugin({
			template: './src/template.html',
		}),
	],
};
```

we can tell the plugin to create the <rootDir>/dist/index.html from a template so if we set up for example the favicon.ico in a template.html
we will have it in <rootDir>/dist/index.html

remove the <rootDir>/index.html and create a html template in <rootDir>/src/template.html

```properties
    npm install --save-dev clean-webpack-plugin
```

clean-webpack-plugin: this plugin will automatically remove the dist folder so a clean dist folder will be created.
During the production build all file with a hash will be generated so the new dist folder won't have any previous hash file unused.

mini-css-extract-plugin:

```properties
    npm install --save-dev mini-css-extract-plugin
```

```js
module.exports = {
	plugins: [
		new CleanwebpackPlugin(),
		new MiniCssExtractPlugin({ filename: '[name].[contentHash].css' }),
	],
	module: {
		rules: {
			test: /\.css$/,
			use: [MiniCssExtractPlugin.loader, 'css-loader'],
		},
	},
};
```

```properties
    npm install --save-dev optimize-css-assets-webpack-plugin
```

This plugin will extract the css from index.js and instead of creating a style tag in the index.html
it will create its own css file. This is a best to do in production because we won't have a first dom displayed
without styling.

#### CASHBUSTING

Cache busting is useful because it allows the visitors to receive the most recently updated files without having to perform a hard refresh or clear their browser cach

on macOs : cmd + shift + R : hard refresh
cmd + R : refresh
With a hard refresh it will force the browser to download all the files even if it's already have them all cached.
A normal refresh will download only the file that have changed ( only if the filename has changed is considered a change )

```js
module.exports = {
	output: {
		filename: 'main.[contenthash].js',
		path: path.resolve(__dirname, 'dist'),
	},
};
```

Anytime a change has been saved in any of the bundled file js, css the contenthash will change then it will force the browser to download the new version of the file with the new hash.
contenthash is a hash generated base on its content.
