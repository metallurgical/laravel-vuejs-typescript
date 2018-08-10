# laravel-vuejs-typescript
Enable Typescript on Laravel 5.6 project with vuejs 2

## Steps
1. Install laravel using laravel command or composer either way should work
2. Install package dependencies:
  - **typescript** which require version ^3.0.1
    - `yarn add typescript` or `yarn add typescript@^3.0.1`
  - **ts-loader** which require version ^3.5.0
    - `yarn add ts-loader@^3.5.1`
  - **vue-class-component**(optional if you want to use class style, i used this one and its really awesome)
    - `yarn add vue-class-component`
3. Adding `webpackConfig` configuration inside `webpack.mix.js` file(optional):



```js

mix.webpackConfig({
    resolve: {
        extensions: ['*', '.js', '.jsx', '.vue', '.ts', '.tsx'],
    },
});

```

4. Change `.js()` into `.ts()` for all compiles methods inside `webpack.mix.js`:

```js
mix
    .sass('resources/assets/sass/app.scss', 'public/css')
    .ts('resources/assets/js/app.ts', 'public/js')
    .webpackConfig({
        resolve: {
            extensions: ['*', '.js', '.jsx', '.vue', '.ts', '.tsx'],
        },
    });
```

5. Rename `resources/assets/js/app.js` to `resources/assets/js/app.ts` and observe following code:
```ts

import "./bootstrap";
import Vue from "vue";
import ExampleComponent from "./components/ExampleComponent.vue";

Vue.component('example-component', ExampleComponent);

const app = new Vue({
    el: '#app'
});
```

6. Add `declaration` module at the root of js folder(`resources/assets/js/`) with filename `typings.d.ts`:

```ts
declare module '*.vue' {
    import Vue from 'vue'
    export default Vue
}
```

7. Adding `tsconfig.json` at the project's root folder:

```js
{
  "compilerOptions": {
    // this aligns with Vue's browser support
    "target": "es5",
    // this enables stricter inference for data properties on `this`
    "strict": true,
    // if using webpack 2+ or rollup, to leverage tree shaking:
    "module": "es2015",
    "moduleResolution": "node",
    "experimentalDecorators": true
  },
  "include": [
    "resources/assets/js/**/*"
  ]
}
```

8. Include `<script src="{{ mix('js/app.js') }}"></script>` tag before body tag of your html file.
9. Sample code inside `.vue` file:

```vue
<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">Example Component</div>

                    <div class="card-body">
                        I'm an example component.
                        <button @click="hey"></button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
    import Vue from "vue"
    import Component from "vue-class-component"

    interface Point {
        color: string;
        height?: number
    }

    @Component
    export default class ExampleComponent extends Vue {

        init(params: Point): void {
            console.log(params.color, params.height);
        }

        hey(): void {
            alert('hohoho');
        }

        mounted(): void {
            console.log('typescript is working')
            this.init({color: 'red', height: 400});
        }
    }
</script>
```

10. Sample code HTML file:

```html
<!doctype html>
<html lang="{{ app()->getLocale() }}">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <meta name="csrf-token" content="{{ csrf_token() }}">

        <title>Laravel</title>

        <!-- Fonts -->
        <link href="https://fonts.googleapis.com/css?family=Raleway:100,600" rel="stylesheet" type="text/css">

        <!-- Styles -->
        <style>
            /** hide for brevity **/
        </style>
    </head>
    <body>
        <div class="flex-center position-ref full-height" id="app">
            @if (Route::has('login'))
                <div class="top-right links">
                    @auth
                        <a href="{{ url('/home') }}">Home</a>
                    @else
                        <a href="{{ route('login') }}">Login</a>
                        <a href="{{ route('register') }}">Register</a>
                    @endauth
                </div>
            @endif

            <div class="content">
                <div class="title m-b-md">
                    Laravel
                </div>

                <div class="links">
                    <a href="https://laravel.com/docs">Documentation</a>
                    <a href="https://laracasts.com">Laracasts</a>
                    <a href="https://laravel-news.com">News</a>
                    <a href="https://forge.laravel.com">Forge</a>
                    <a href="https://github.com/laravel/laravel">GitHub</a>
                </div>

                <example-component></example-component>


            </div>
        </div>
        <script src="{{ mix('js/app.js') }}"></script>
    </body>
</html>
```
11. Lastly, build source file either using `npm run dev` or `npm run watch`
12. Have fun!
