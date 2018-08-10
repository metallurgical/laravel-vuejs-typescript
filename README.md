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

5. Rename `resources/assets/js/app.js` to `resources/assets/js/app.ts`
6. Add `declaration` module at the root of assets folder(`resources/assets/js/`) with filename `typings.d.ts`:

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

8. Sample code inside `.vue` file:

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
