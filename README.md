# @vitejs/plugin-vue2-jsx [![npm](https://img.shields.io/npm/v/@vitejs/plugin-vue2-jsx.svg)](https://npmjs.com/package/@vitejs/plugin-vue2-jsx)

[中文文档](./README.zh-CN.md)

> [!NOTE]
> This is a community-maintained fork of the official [@vitejs/plugin-vue2-jsx](https://github.com/vitejs/vite-plugin-vue2-jsx), which has been abandoned by the Vue team. This fork adapts the plugin to work with **Vite 8**, so that Vue 2 projects can also benefit from the performance improvements brought by the latest Vite version.

> [!CAUTION]
> Vue 2 has reached EOL, and the official plugin is no longer actively maintained.

---

Provides Vue 2 JSX & TSX support with HMR, requires Vite 8.

```js
// vite.config.js
import vueJsx from '@vitejs/plugin-vue2-jsx'

export default {
  plugins: [
    vueJsx({
      // options are passed on to @vue/babel-preset-jsx
    })
  ]
}
```

## What's Changed in v2.0.0

- Requires Vite 8 (`peerDependencies` updated)
- Switched JSX/TSX transform from `esbuild` to `oxc` (Vite 8 replaced esbuild with OXC as the default transformer)

## Options

### include

Type: `(string | RegExp)[] | string | RegExp | null`

Default: `/\.[jt]sx$/`

A [picomatch pattern](https://github.com/micromatch/picomatch), or array of patterns, which specifies the files the plugin should operate on.

### exclude

Type: `(string | RegExp)[] | string | RegExp | null`

Default: `undefined`

A [picomatch pattern](https://github.com/micromatch/picomatch), or array of patterns, which specifies the files to be ignored by the plugin.

> See [@vue/babel-preset-jsx](https://github.com/vuejs/jsx-vue2/tree/dev/packages/babel-preset-jsx#readme) for other options.

## HMR Detection

This plugin supports HMR of Vue JSX components. The detection requirements are:

- The component must be exported.
- The component must be declared by calling `defineComponent` via a root-level statement, either variable declaration or export declaration.

### Supported patterns

```jsx
import { defineComponent } from 'vue'

// named exports w/ variable declaration: ok
export const Foo = defineComponent({})

// named exports referencing variable declaration: ok
const Bar = defineComponent({ render() { return <div>Test</div> }})
export { Bar }

// default export call: ok
export default defineComponent({ render() { return <div>Test</div> }})

// default export referencing variable declaration: ok
const Baz = defineComponent({ render() { return <div>Test</div> }})
export default Baz
```

### Non-supported patterns

```jsx
// not using `defineComponent` call
export const Bar = { ... }

// not exported
const Foo = defineComponent(...)
```

## Unfinished Features

- SSR support
- Share the same HMR runtime with `@vitejs/plugin-vue2`
