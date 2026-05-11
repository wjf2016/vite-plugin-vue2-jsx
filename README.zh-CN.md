# @vitejs/plugin-vue2-jsx [![npm](https://img.shields.io/npm/v/@vitejs/plugin-vue2-jsx.svg)](https://npmjs.com/package/@vitejs/plugin-vue2-jsx)

[English](./README.md)

> [!NOTE]
> 这是一个社区维护的 fork 版本，源自官方 [@vitejs/plugin-vue2-jsx](https://github.com/vitejs/vite-plugin-vue2-jsx)。官方插件已被 Vue 团队放弃维护，无法兼容 Vite 8。本 fork 对插件进行了修改和适配，使 Vue 2 项目也能使用 **Vite 8**，享受最新 Vite 版本带来的性能优势。

> [!CAUTION]
> Vue 2 已经停止维护（EOL），官方插件也不再更新。

---

为 Vue 2 提供 JSX & TSX 支持，包含 HMR 热更新功能，要求使用 Vite 8。

```js
// vite.config.js
import vueJsx from '@vitejs/plugin-vue2-jsx'

export default {
  plugins: [
    vueJsx({
      // 选项会传递给 @vue/babel-preset-jsx
    })
  ]
}
```

## v2.0.0 变更说明

- `peerDependencies` 要求 Vite 8
- JSX/TSX 转换由 `esbuild` 切换为 `oxc`（Vite 8 使用 OXC 替代 esbuild 作为默认转换器）

## 选项

### include

类型：`(string | RegExp)[] | string | RegExp | null`

默认值：`/\.[jt]sx$/`

[picomatch](https://github.com/micromatch/picomatch) 匹配模式，指定插件需要处理的文件。

### exclude

类型：`(string | RegExp)[] | string | RegExp | null`

默认值：`undefined`

[picomatch](https://github.com/micromatch/picomatch) 匹配模式，指定插件需要忽略的文件。

> 其他选项请参阅 [@vue/babel-preset-jsx](https://github.com/vuejs/jsx-vue2/tree/dev/packages/babel-preset-jsx#readme)。

## HMR 热更新检测

本插件支持 Vue JSX 组件的 HMR 热更新。检测要求如下：

- 组件必须被导出。
- 组件必须通过 `defineComponent` 声明，且位于顶层语句（变量声明或导出声明）。

### 支持的写法

```jsx
import { defineComponent } from 'vue'

// 具名导出 + 变量声明：支持
export const Foo = defineComponent({})

// 具名导出引用变量声明：支持
const Bar = defineComponent({ render() { return <div>Test</div> }})
export { Bar }

// 默认导出调用：支持
export default defineComponent({ render() { return <div>Test</div> }})

// 默认导出引用变量声明：支持
const Baz = defineComponent({ render() { return <div>Test</div> }})
export default Baz
```

### 不支持的写法

```jsx
// 未使用 defineComponent
export const Bar = { ... }

// 未导出
const Foo = defineComponent(...)
```

## 未完成功能

- SSR 支持
- 与 `@vitejs/plugin-vue2` 共享 HMR 运行时
