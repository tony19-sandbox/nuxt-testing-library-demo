# How to Add Test Support in Nuxt Project

This project outlines how to add [Jest](https://jestjs.io/) unit testing support to an already existing [Nuxt](https://nuxtjs.org/) project.

## Project Scaffolding

The base project was scaffolded with [`create-nuxt-app`](https://nuxtjs.org/docs/2.x/get-started/installation/#using-create-nuxt-app) using the following settings:

>     $ npx create-nuxt-app nuxt-testing-library-demo
>
>     create-nuxt-app v3.5.2
>     ✨  Generating Nuxt.js project in nuxt-testing-library-demo
>     ? Project name: nuxt-testing-library-demo
>     ? Programming language: JavaScript
>     ? Package manager: Yarn
>     ? UI framework: None
>     ? Nuxt.js modules: (Press <space> to select, <a> to toggle all, <i> to invert se
>     lection)
>     ? Linting tools: (Press <space> to select, <a> to toggle all, <i> to invert sele
>     ction)
>     ? Testing framework: None
>     ? Rendering mode: Single Page App
>     ? Deployment target: Static (Static/JAMStack hosting)
>     ? Development tools: jsconfig.json (Recommended for VS Code if you're not using
>     typescript)
>     ? What is your GitHub username? tony trinh
>     ? Version control system: Git

## Adding Jest support

The steps below are based on the [`jest` template from `@nuxt/create-nuxt-app`](https://github.com/nuxt/create-nuxt-app/tree/d00a766/packages/cna-template/template/frameworks/jest). It installs [*Vue Test Utils*](), but you could replace that with [*Vue Testing Library*](https://testing-library.com/docs/vue-testing-library/intro/) (which transitively pulls in *Vue Test Utils*) if preferred.

 1. Install the prerequisite NPM packages for `@vue/test-utils` and `jest`:

    ```
    npm install -D @vue/test-utils \
                   vue-jest@^3 \
                   jest@^26 \
                   babel-core@7.0.0-bridge.0 \
                   babel-jest@^26

    npm install -D ts-jest@^26 # if using TypeScript
    ```

 2. Add an NPM script to run Jest CLI:

    ```js
    // <rootDir>/package.json
    {
      "scripts": {
        "test": "jest"
      }
    },
    ```

 3. Add a Jest config:

    ```js
    // <rootDir>/jest.config.js
    module.exports = {
      moduleNameMapper: {
        '^@/(.*)$': '<rootDir>/$1',
        '^~/(.*)$': '<rootDir>/$1',
        '^vue$': 'vue/dist/vue.common.js'
      },
      moduleFileExtensions: [
        'ts', // if using TypeScript
        'js',
        'vue',
        'json'
      ],
      transform: {
        "^.+\\.ts$": "ts-jest", // if using TypeScript
        '^.+\\.js$': 'babel-jest',
        '.*\\.(vue)$': 'vue-jest'
      },
      collectCoverage: true,
      collectCoverageFrom: [
        '<rootDir>/components/**/*.vue',
        '<rootDir>/pages/**/*.vue'
      ]
    }
    ```

  4. Add a Babel config:

     ```js
     // <rootDir>/.babelrc
     {
       "env": {
         "test": {
           "presets": [
             [
               "@babel/preset-env",
               {
                 "targets": {
                   "node": "current"
                 }
               }
             ]
           ]
         }
       }
     }
     ```

  5. Add a `test` directory to contain unit test files, and add the example `.spec.js` shown below. Note the location of the test files can be configured with the [`testMatch`](https://jestjs.io/docs/configuration#testmatch-arraystring) or [`testRegex`](https://jestjs.io/docs/configuration#testregex-string--arraystring) setting in `jest.config.js`.

     For example:

     ```js
     // <rootDir>/test/Logo.spec.js
     import { mount } from '@vue/test-utils'
     import Logo from '@/components/Logo.vue'

     describe('Logo', () => {
       test('is a Vue instance', () => {
         const wrapper = mount(Logo)
         expect(wrapper.vm).toBeTruthy()
       })
     })
     ```
