## Vue 技术学习

在本次项目中，前端将采用 Vue 框架实现。

Vue.js 是一个提供了 MVVM 风格的双向数据绑定的 Javascript 库。它的核心是 MVVM 中的 VM，也就是 ViewModel。 ViewModel 实现数据和视图的一致性。

1. Vue 实例属性

   `el`：模板对象

   `data`：数据对象；当这些数据改变时，视图会进行重渲染。也可通过改变视图改变 data 属性值

   `methods`：函数对象

   `compute`：不同于函数对象，会保存计算结果。当计算属性的依赖数据发生改变时才重新计算

   `watch`：监听数据的变化，执行相应函数

2. Vue 生命周期

   - 创建阶段

     对应钩子：`BeforeCreate`、`created`

   - 挂载阶段

     编译模板，实现模板的挂载

     对应钩子：`beforeMount`、`mounted`

   - 更新阶段

     当数据发生改变或视图引起数据改变时需要更新

     对应钩子：`beforeUpdate`、`updated`

   - 销毁阶段

     销毁实例（通过调用 `vm.$destroy()`）

     对应钩子：`beforeDestroy`、`destroyed`

3. 模板语法

   - **文本插值**，双大括号语法

     一次性插值：v-once

   - **原始 HTML **

     双大括号将数据解释为普通文本，为输出HTML，使用 v-html 指令。

     ```html
     <span v-html="rawHtml"></span></p>
     ```

     这个 `span` 的内容将会被替换成为属性值 `rawHtml`，直接作为 HTML。

   - **特性**

     Mustache 语法不能作用在 HTML 特性上，应该使用 v-bind 指令：

     ```html
     <div v-bind:id="dynamicId"></div>
     ```

   - **使用JS表达式**

     对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

     每个绑定都只能包含**单个表达式**

     模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 `Math` 和 `Date` 。你不应该在模板表达式中试图访问用户定义的全局变量。

4. 值绑定 v-bind

   使用 v-bind 把值绑定到 Vue 实例的一个动态属性上，并且这个属性的值可以不是字符串。

   ```html
   <elem v-bind:attr="attr"></elem>
   ```

5. 表单输入绑定 v-model

   使用 v-model 实现表单元素的双向绑定。

   v-model 本质上是一个语法糖，它负责监听用户的输入事件以更新数据。

   ```html
   <input type="..." v-model="data"></input>
   ```

6. 事件处理 v-on

   用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。`v-on` 还可以接收一个需要调用的方法名称。

   ```html
   <elem v-on:someEvent="method"></elem>
   ```

7. 组件

   所有的 Vue 组件同时也都是 Vue 的实例，可接受相同的选项对象 (除了一些根级特有的选项) 并提供相同的生命周期钩子。组件的 data 属性必须是函数。

   父组件模板中同样可以使用子组件，组件实例的作用域是**孤立的**。父组件通过 **prop** 给子组件下发数据，子组件通过**事件**给父组件发送消息。

   Prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是反过来不会。

   可以使用数组方式传递 prop，也可为组件的 prop 指定验证规则。

   prop 会在组件实例创建**之前**进行校验，所以在 `default` 或 `validator` 函数里，诸如 `data`、`computed` 或 `methods` 等实例属性还无法使用。

   可以通过自定义事件实现子组件跟父组件通信。在子组件中触发 (`vm.$emit()`) 自定义事件，在父组件中监听(`v-on`) 该事件，还可实现数据的传递（不能用 `$on` 监听子组件释放的事件，而必须在模板里直接用 `v-on` 绑定）。也可让组件监听原生事件。

   父组件使用插槽分发内容，在子组件中使用 `slot`，父组件则可将内容传递到该 `slot` 中去，最终显示在页面上。可以给插槽命名，即具名插槽，实现更精准的传递。

   还可使用动态组件，通过使用保留的 `<component>` 元素，并对其 `is` 特性进行动态绑定，可以在同一个挂载点动态切换多个组件。

8. 过滤器

   过滤器可用在两个地方：双花括号插值和 v-bind 表达式。

   过滤器是 js 函数，总接收表达式的值 (之前的操作链的结果) 作为第一个参数。过滤器可以串联，也可接收更多参数。

   过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示：

   ```html
   <!-- 在双花括号中 -->
   {{ message | capitalize }}

   <!-- 在 `v-bind` 中 -->
   <div v-bind:id="rawId | formatId"></div>
   ```

   可以通过 `Vue.filter('name', function(){})`定义全局过滤器，或者在 Vue 实例对象中定义本地过滤器。

9. Vue-router

   创建 vue-router 实例将组件(components)映射到路由(routes)，并将其注入Vue实例。

   1. `<router-link>`

      支持用户在具有路由功能的应用中（点击）导航。 通过 `to` 属性指定目标地址，默认渲染成带有正确链接的 `<a>` 标签，可以通过配置 `tag` 属性生成别的标签.。

   2. `<router-view>`

      渲染路径匹配到的视图组件。`<router-view>` 渲染的组件还可以内嵌自己的 `<router-view>`，根据嵌套路径，渲染嵌套组件。

   3. 动态路由匹配

      把某种模式匹配到的所有路由，全都映射到同个组件。

      通过在 `vue-router` 的路由路径中使用『动态路径参数』（dynamic segment）来达到这个效果。

      ```javascript
      routes: [
          // 动态路径参数 以冒号开头
          { path: '/user/:id', component: User }
      ]
      ```

      当匹配到一个路由时，参数值会被设置到 `this.$route.params`，可以在每个组件内使用。

      可以在一个路由中设置多段『路径参数』，对应的值都会设置到 `$route.params` 中。

      当使用路由参数时，**原来的组件实例会被复用**,组件的生命周期钩子不会再被调用。复用组件时，想对路由参数的变化作出响应，可以 watch（监测变化） `$route` 对象。

   4. 嵌套路由

      要在嵌套的出口中渲染组件，需要在 `VueRouter` 的参数中使用 `children` 配置：

      ```javascript
      routes: [
          { path: '/user/:id', component: User,
            children: [
              {
                path: 'profile',
                component: UserProfile
              },
              {
                path: 'posts',
                component: UserPosts
              }
            ]
          }
      ]
      ```

   5. 编程式导航

      可以借助 router 的实例方法来定义导航链接，通过编写代码来实现：

      `router.push(location, onComplete?, onAbort?)`

      在 Vue 实例内部，可以通过 \$router 访问路由实例: `this.$router.push`

      点击 `<router-link :to="...">` 等同于调用 `router.push(...)`。

10. vuex

    Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

    Vuex 使用**单一状态树**。相当于把组件的共享状态抽取出来，以一个全局单例模式管理。

    在 store 中保存状态（或者可以简单理解为保存数据），更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。

    Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数。

    不能直接调用一个 mutation handler。当触发一个类型为 `increment` 的 mutation 时，调用此函数。mutation 必须是同步函数。

    Action 类似于 mutation，不同在于：

    - Action 提交的是 mutation，而不是直接变更状态。
    - Action 可以包含任意异步操作。

    Action 通过 `store.dispatch` 方法触发。

    ​

    ​
