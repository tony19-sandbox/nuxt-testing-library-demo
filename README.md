> Demo for using Nuxt with `@testing-library/vue`

https://stackoverflow.com/q/66637556/6277151

## Project generated

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

### Adding Jest support

The following steps demonstrate what [`create-nuxt-app` does to add Jest support](https://github.com/nuxt/create-nuxt-app/tree/d00a766/packages/cna-template/template/frameworks/jest).

 1. Install the prerequisite NPM packages for `@vue/test-utils` and `jest`:

   ```
   npm install -D @vue/test-utils@^1 \
                  vue-jest@^3 \
                  jest@^26 \
                  babel-core@7.0.0-bridge.0 \
                  babel-jest@^26

   npm install -D ts-jest@^26 # if using TypeScript
   ```

 3. Add an NPM script to run Jest CLI:

    ```js
    // <rootDir>/package.json
    {
      "scripts": {
        "test": "jest"
      }
    },
    ```

 2. Add a Jest config:

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

  3. Add a Babel config:

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

  4. Add a `test` directory to contain unit tests. For example:

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