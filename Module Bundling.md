# __Module Bundling__

- [__Module Bundling__](#module-bundling)
- [Bundling Setup](#bundling-setup)
  - [Webpack Setup](#webpack-setup)
    - [webpack.config.js](#webpackconfigjs)
  - [Babel Setup](#babel-setup)
    - [.babelrc](#babelrc)
  - [Eslint Setup](#eslint-setup)
    - [.eslintrc.json](#eslintrcjson)
  - [Optional Config Tool](#optional-config-tool)
- [Parcel](#parcel)
- [Rollup.js](#rollupjs)

# Bundling Setup

## Webpack Setup

- Add package.json to project

  ``npm init -y``

- Create ``dist`` folder in root directory

- Create ``index.html`` file in ``dist`` folder

  ```html
  <!DOCTYPE html>
  <html>

  <head>
    <title>Webpack</title>
  </head>

  <body>
    <div id='app'></div>
    <script src='/bundle.js'></script>
  </body>

  </html>
  ```


- Install npm packages for webpack as developer dependencies

  ``npm i -D webpack webpack-dev-server webpack-cli``

- Add webpack-dev-server script to ``package.json`` scripts

  ```json
  "scripts": {
    "start": "webpack-dev-server --config ./webpack.config.js --mode development"
  },
  ```

- Create ``webpack.config.js`` in ``root`` directory
  ```javascript
  module.exports = {
    entry: [
      './src/index.js'
    ],
    output: {
      path: __dirname + '/dist',
      publicPath: '/',
      filename: 'bundle.js'
    },
    devServer: {
      contentBase: './dist'
    }
  }
  ```
- Create ``src`` folder in ``root`` directory
- Create ``index.js`` file in ``src`` folder
- Add anything to the ``index.js`` file
- Run ``npm start`` in the root directory

### webpack.config.js

```javascript

```

## Babel Setup

- Install ``babel`` packages as development dependencies in the project

  ``npm i -D @babel/core @babel/preset-env @babel/preset-react babel-loader``

- Create a ``.babelrc`` file in the root directory

  ```json
  {
    "presets": [
      "@babel/preset-env",
      "@bable/preset-react"
    ],
    "plugins": []
  }
  ```

- Add babel config to the ``webpack.config.js`` file

  ```javascript
  //...
  module: {
    rules: [
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['babel-loader']
    },
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx']
  }
  ```


### .babelrc

```json
{
  "presets": [
    "@babel/preset-env",
    "@bable/preset-react"
  ],
  "plugins": []
}
```

## Eslint Setup

- Install ``Eslint`` packages as development dependencies

  ``npm i -D eslint eslint-loader``

- Add eslint config below the babel config in the ``webpack.config.js`` file

  ```javascript
    //...
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['eslint-loader']
    },
  ```

  - Add ``.eslintrc.json`` file to the root directory

  ```json
  {
    "rules": {

    }
  }
  ```

- Install ``babel-eslint`` package into the the development dependencies

  ``npm i -D babel-eslint``

- Add ``parser`` property into the ``.eslintrc.json`` file

  ```json
  {
  "parser": "babel-eslint",
  "rules": {}
  }
  ```

- Add any rules wanted to the ``rules`` section of the ``.eslintrc.json``

  ```json
  {
    "parser": "babel-eslint",
    "rules": {
      "no-console": 1
    }
  }
  ```

- Install any optional plugins or style guides for ``eslint``

  ``npm i -D eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y``

### .eslintrc.json

```json
{
  "parser": "babel-eslint",
  "rules": {},
  "extends": ["airbnb-base"]
}
```

## Optional Config Tool
   - [ https://webpack.js.org/](https://createapp.dev/)

# Parcel

- #### For small projects
  https://parceljs.org/

- Install parcel

  ``npm init -y``

  ``npm i -D parcel-bundler``

- Install babel packages as developer dependencies

  - [Babel Setup](#babel-setup)

- Add parcel script to ``package.json`` scripts

  ```json
  "scripts": {
    "start": "parcel ./src/index.html"
  },
  ```

- Create ``src`` folder in root directory

- Create ``index.html`` file in ``src`` folder

  ```html
  <!DOCTYPE html>
  <html>

  <head>
    <title>Webpack</title>
  </head>

  <body>
    <div id='app'></div>
    <script src='/index.js'></script>
  </body>

  </html>
  ```

- run `npm start` to create the `dist` directory and bundle files


# Rollup.js

- #### For personal NPM package rollout
  https://rollupjs.org/guide/en/