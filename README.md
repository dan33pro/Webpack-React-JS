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


## Optimización de Webpack para React

1. Insatlamos las dependencias

    ```npm
    npm install css-minimizer-webpack-plugin terser-webpack-plugin -D
    ```

2. Creamos un archivo `webpack.config.dev.js` con lo mismo que el [webpack.config.js](https://github.com/dan33pro/Webpack-React-JS/blob/main/webpack.config.js)
pero le agregamos la linea 

    ```js
    mode: 'development',
    ```

    Y en webpack.config.js](https://github.com/dan33pro/Webpack-React-JS/blob/main/webpack.config.js) dejamos

    ```js
    mode: 'production',
    ```

3. Ahora en nuestro archivo [webpack.config.js](https://github.com/dan33pro/Webpack-React-JS/blob/main/webpack.config.js) vamos a quitar la propiedad `devServer` que no va a producción y agregamos en la propiedad `output` dentro `clean: true,`

4. Agregamos las constantes siguientes en este mismo archivo

    ```js
    const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
    const TerserWebpackPlugin = require('terser-webpack-plugin');
    ```

    También los alías para los `path` dentro de la propiedad `resolve` en ambos archvos de configuración

    ```js
    alias: {
        '@components': path.resolve(__dirname, 'src/components/'),
        '@styles': path.resolve(__dirname, 'src/styles/'),
    },
    ```

5. En [webpack.config.js](https://github.com/dan33pro/Webpack-React-JS/blob/main/webpack.config.js) agregamos debajo de `plugins`

    ```js
    optimization: {
        minimize: true,
        minimizer: [
            new CssMinimizerPlugin(),
            new TerserWebpackPlugin(),
        ],
    },
    ```

6. Modificamos los scripts en el [package.json](https://github.com/dan33pro/Webpack-React-JS/blob/main/package.json)

    ```json
    "start": "webpack serve --config webpack.config.dev.js",
    "build": "webpack --config webpack.config.js",
    "dev": "webpack --config webpack.config.js"
    ```

## De React 17 a React 18

`ReactDom.render` ya no es compatible con `React 18` en su lugar en el archivo `index.js` ponemos

    ```js
    import React from "react";
    import { createRoot } from "react-dom/client";
    import App from './components/App';
    import './styles/global.scss';

    const container = document.getElementById('app');
    const root = createRoot(container);
    root.render(<App tab="home" />);
    ```

## Adicionales no necesarios

Voy añadir un poco más de estilos, porque quiero jeje, solo modifique un poco el Dom en el archivo `App.jsx`

    ```jsx
    import React from "react";

    const title = <h1>Hola react!!!</h1>
    const text = <span>Nunca he trabajado con React, solo estoy haciendo experimentos, espero 
        pronto poder hacer el curso de React.js</span>;

    const App = () => <div className="container">{title}{text}</div>;
 
    export default App;
    ```

Y en el archivo [global.scss](https://github.com/dan33pro/Webpack-React-JS/blob/main/src/styles/global.scss) voy a agregar

    ```scss
    $base-color: #b99ba9;
    $shadow-dark: #746e71;
    $shadow-light: #eee1e7;
    $contrast-clor: #FFFFFF;
    $color: rgba(black, 0.88);

    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }
    body {
        width: 100vw;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: $base-color;
        color: $color;
    }

    #app {
        width: 600px;
        height: 300px;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        padding: 20px 60px;
        background-color: $contrast-clor;
        border-radius: 20px;
        box-shadow: 10px 10px 15px rgba($shadow-dark, 0.5),
                    5px 5px 10px rgba($shadow-dark, 0.75),
                    -10px -10px 15px rgba($shadow-light, 0.5);
    }

    h1 {
        text-align: center;
        padding: 10px 5px;
    }
    ```


## Deploy del proyecto con React.js

El ultimo paso es el despliegue, para esto usamos `Netlify`, el [resultado](https://63419a31a145c458bdad3acf--remarkable-kitsune-64db81.netlify.app/).

