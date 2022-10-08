# Integración básica de React.js
Esta es la continuación de esta saga de 3 repos en los que venimos trabajando con Webpack

## Instalación y configuración de React.js

1. Para comenzar lo primero es instalar las dependecias de react con

    ```npm
    npm install react react-dom
    ```
2. Iniciamos la configuración basica de npm

    ```npm
    npm init -y
    ```
3. Para la estructura inicial de nuestro proyecto

    - Ceramos una carpeta `public` en la raíz con un archivo `index.html`

    ```html
    <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Hola React!</title>
        </head>
        <body>
            <div id="app"></div>
        </body>
        </html>
    ```

    - Ahora también en la raiz un directorio `src` con un archivo `index.js`

    ```js
    import React from "react";
    import ReactDOM from "react-dom";
    import App from './components/App';

    ReactDOM.render(<App />, document.getElementById('app'));
    ```

    Acá importamos las dependencias que instalamos, `react` y `react-dom` e importamos
    un componente que no hemos creado, y le decimos donde lo renderice.

    - Para nuestro componente, creamos un drcetorio `components` dentro de `src` con
    un archivo `App.jsx`

    ```jsx
    import React from "react";

    const App = () => <h1>Hola react!</h1>;

    export default App;
    ```

## Configuración de Webpack 5 para React.js

1. Instalamos las dependencias necesarias

    ```npm
    npm install @babel/core @babel/preset-env @babel/preset-react babel-loader -D
    ```

2. Añadimos el archivo `.babelrc` en la raíz

    ```json
    {
        "presets": [
            "@babel/preset-env",
            "@babel/preset-react"
        ]
    }
    ```

3. Insatamos las dependecias para `Webpack`

    ```npm
    npm install webpack webpack-cli webpack-dev-server -D
    ```

4. Creamos nuestro archivo de configuración [webpack.config.js](https://github.com/dan33pro/Webpack-React-JS/blob/main/webpack.config.js)

    ```js
    const path = require('path');

    module.exports =  {
        entry: './src/index.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'bundle.js',
        },
        resolve: {
            extensions: ['.js', '.jsx'],
        },
        module: {
            rules: [
                {
                    test: /\.(js|jsx)$/,
                    exclude: /node_modules/,
                    use: {
                        loader: 'babel-loader',
                    },
                },
            ],
        },
        devServer: {
            static:  path.join(__dirname, 'dist'),
            compress: true,
            port: 3006,
        },
    }
    ```

## Configuración de plugins y loaders para React

1. Insatlamos las dependencias

    ```npm
    npm install html-loader html-webpack-plugin -D
    ```

2. En el archivo [webpack.config.js](https://github.com/dan33pro/Webpack-React-JS/blob/main/webpack.config.js) añadimos

    ```js
    const HtmlWebpackPlugin = require('html-webpack-plugin');
    ```

    Agregamos una nueva regla en `rules`

    ```js
    {
        test: /\.html$/,
        use: [
            { loader: 'html-loader' },
        ],
    },
    ```

    Y agregamos el array de `plugins` como propiedad de `module.exports`

    ```js
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            filename: './index.html',
        }),
    ],
    ```
3. Agregamos los scripts necesarios en el [package.json](https://github.com/dan33pro/Webpack-React-JS/blob/main/package.json)

    ```json
    "start": "webpack serve",
    "build": "webpack --mode production",
    "dev": "webpack --mode development"
    ```

    Con esto ya tenemos nuestro local server para desarrollar

## Configuración de Webpack para CSS en React

1. Instalamos las siguientes dependencias

    ```npm
    npm install mini-css-extract-plugin css-loader style-loader sass sass-loader -D
    ```

2. En nuestro archivo de [webpack.config.js](https://github.com/dan33pro/Webpack-React-JS/blob/main/webpack.config.js) añadimos la configuración para estas
dependencias.

    ```js
    const MiniCssExtractPlugin = require('mini-css-extract-plugin');
    ```

    luego la nueva regla que agregamos al arreglo `rules`

    ```js
    {
        test: /\.s[ac]ss$/,
        use: [
            'style-loader',
            'css-loader',
            'sass-loader',
        ],
    }
    ```

    Y el nuevo elemento del array de `plugins`

    ```js
    new MiniCssExtractPlugin({
        filename: '[name].css'
    }),
    ```

3. Ya con esto podemos probar el resultado ceando un archivo [global.scss](https://github.com/dan33pro/Webpack-React-JS/blob/main/src/styles/global.scss) con

    ```scss
    $base-color: #c6538c;
    $color: rgba(black, 0.88);

    body {
        background-color: $base-color;
        color: $color;
    }
    ```

4. Lo mportamos en el [index.js](https://github.com/dan33pro/Webpack-React-JS/blob/main/src/index.js) 

    ```js
    import './styles/global.scss';
    ```

5. Corremos con `npm run start`