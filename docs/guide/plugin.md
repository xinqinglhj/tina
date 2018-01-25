# 插件
为了让你能够更轻松地使用扩展功能，Tina 提供了一套与 [**Vue** 相似的](https://cn.vuejs.org/v2/guide/plugins.html) 插件机制。

## 安装插件
使用 ``Tina.use`` 方法安装指定插件：

```javascript
/**
 * /app.js or <script> in /app.mina
 */
import Tina from '@tinajs/tina'
import loading from '@tinajs/tina-loading'
import modal from '@tinajs/tina-modal'

Tina.use(loading)
Tina.use(modal, { alertTitle: 'Hey!' })

App({
  // ...
})
```

在重复安装相同插件时，``Tina.use`` 将保证插件只被安装一次。


## 开发插件
与 [Vue 的插件机制](https://cn.vuejs.org/v2/guide/plugins.html#开发插件) 相似，Tina 插件的范围没有任何限制。例如你可以在插件中使用混合，或者直接添加全局属性、方法等。

我们约定 Tina 的插件对象应实现 ``install`` 方法。该方法接受第一个参数是 ``Tina`` 类，你可以从中访问 ``Page``、``Component`` 等变量；剩余参数均为可选，并与用户调用 ``Tina.use`` 时第二个起的所有参数一致。

例如：
```javascript
/**
 * /libraries/my-plugin.js
 */
const MyPlugin = {
  // ...
}

MyPlugin.install = function (Tina, options) {
  const { Page, Component } = Tina

  /**
   * 使用混合
   */

  Page.mixin({
    // ...
  })

  Component.mixin({
    // ...
  })
}

export default MyPlugin
```

```javascript
/**
 * /app.js or <script> in /app.min
 */
import Tina from '@tinajs/tina'
import MyPlugin from './libraries/my-plugin'

const options = {
  // ...
}

Tina.use(MyPlugin, options)

App({
  // ...
})
```