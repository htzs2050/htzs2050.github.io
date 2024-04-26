1. get请求

        用于获取数据。

2. post请求

        用于提交数据（新建）、包括表单提交及文件上传。

3. put请求

        用于更新数据（修改），将所有数据都推送到后端。

4. patch请求

        用于更新数据（修改），只将修改的数据推送到后端。

5. delete请求

        用于删除数据。

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2024-04-26-12-45-36-image.png)

你:

状态管理器和本地存储的区别

### 状态管理器（如 Vuex, Redux）

1. **目的和用途**：
   
   - 状态管理器主要用于管理应用的状态（数据），特别是在复杂的单页面应用（SPA）中，它帮助开发者管理跨多个组件共享的状态，实现状态的集中管理和通信。

2. **生命周期**：
   
   - 状态管理器中存储的数据通常在页面刷新时重置，其生命周期仅限于应用当前的会话。

3. **数据流**：
   
   - 状态管理器支持可预测的数据流，通常都有明确的方式处理状态的更新（如通过 actions, mutations），这有助于维护数据的一致性和调试应用。

### 本地存储（LocalStorage, SessionStorage）

1. **目的和用途**：
   
   - 本地存储是用来在用户的浏览器中持久化数据，使得数据能够在浏览器的不同访问会话之间持续有效。常用于保存用户偏好设置、登录状态、访问令牌等。

2. **生命周期**：
   
   - **LocalStorage** 没有过期时间，数据会一直保存在用户的设备上，直到被显式清除，或者用户清空浏览器缓存。
   - **SessionStorage** 的数据仅在当前浏览器窗口或标签页的会话中有效，窗口或标签页关闭后，数据自动被清空。

3. **数据容量**：
   
   - 本地存储提供更大的存储空间（约 5MB）与Cookies相比，而且只有在浏览器中保存，不像Cookies那样随每次HTTP请求发送到服务器。

4. **安全性**：
   
   - 仅存储在本地，不涉及数据在网络中的传输，减少了数据泄露的风险。但是，数据未经加密且易于通过客户端脚本访问，可能面临跨站脚本攻击（XSS）的风险。

### 比较与选择

- 使用**状态管理器**更适合管理应用的内部状态，特别是需要跨多个组件共享的状态。
- 使用**本地存储**适合进行少量数据的持久化存储，特别是用户凭据或偏好等需要跨会话存储的数据。

在实际应用中，往往会结合使用这两种技术，例如使用状态管理器来控制和管理当前用户状态，同时使用本地存储来持久化用户的登录信息或其他需要长期保存的数据。这样可以有效地利用各自的优势，提高应用的性能和用户体验。

Token\cookie\Session

Session一般用于长时间的推流，比如视频网站、在线语音识别

Cookie只存在本地，之前做信息系统实践的时候做过这个加密，然后传服务端，同数据库里被加密的东西做验证

Token是服务器和本地都存，存在数据库中。一般不能直接被开发者访问。

```typescript
const state = () => ({
    loginVisible: false,
    loginIndex: "1",
    accessToken: localStorage.getItem("accessToken") || "", //access改到-
    refreshToken: localStorage.getItem("refreshToken") || "",
    userId: localStorage.getItem("userId") || 0,
    username: localStorage.getItem("username") || "",
    nickname: localStorage.getItem("nickname") || "",
    name: localStorage.getItem("name") || "",
    email: localStorage.getItem("email") || "",
    qq: localStorage.getItem("qq") || "",
    accessExp: localStorage.getItem("accessExp") || 0,
    refreshExp: localStorage.getItem("refreshExp") || 0,
})这里的getItem是干什么的
```

`localStorage.getItem()` 方法是 Web Storage API 的一部分，该方法用于从浏览器的本地存储（localStorage）中读取存储的数据。`localStorage` 允许网站存储关键值对数据，这些数据会存储在用户的浏览器中，并且即使关闭浏览器后也不会丢失，除非用户主动清除浏览器数据或者由于浏览器的策略原因而被清除。



在 Vue.js 中，当使用 Vuex 作为状态管理库的时候，可以使用模块化的方式来组织代码。下面的代码是一个 Vuex 模块的基础结构，具体包括 `namespaced`, `state`, `getters`, `mutations`, `actions` 等不同部分，每个部分都承担不同的职责：

1. **`namespaced`:** 当设置为 `true` 时，该模块的所有成员（状态、getter、mutations 和 actions）都将被更加独立和封装地注册在全局 store 中。这意味着你通过名称空间访问这个模块的状态和方法，例如访问状态 `store.state['moduleName'].stateProperty` 或调用方法 `store.dispatch('moduleName/actionMethod')`。

2. **`state`:** 这是模块的状态对象，用来存储模块的状态数据。状态可以是任何类型的数据，如字符串、数字、对象等。这里应该定义数据的初始状态。
   
   10px12px14px16px18px`const state = {     count: 0 };`

3. **`getters`:** 类似于组件内的计算属性，getters 是基于 store 中的基本状态来计算派生状态的方法集合。可以用来声明性地获取数据，也会对数据做缓存处理，只有依赖的基本状态发生改变时，才会重新计算。
   
   10px12px14px16px18px`const getters = {     doubleCount(state) {         return state.count * 2;     } };`

4. **`mutations`:** 用于同步变更 store 中的状态。mutation 必须是同步函数。通过调用 `store.commit` 方法来触发 mutation。
   
   10px12px14px16px18px`const mutations = {     increment(state, payload) {         state.count += payload.amount;     } };`

5. **`actions`:** 用于提交 mutation，而不是直接变更状态。actions 可以包含任意异步操作。通过调用 `store.dispatch` 方法来触发 action。这是管理异步操作的理想位置。
   
   10px12px14px16px18px`const actions = {     incrementAsync({ commit }, payload) {         setTimeout(() => {             commit('increment', payload);         }, 1000);     } };`

在这些部分配合下，Vuex 允许我们以一种组织性很强的方式来管理 Vue 应用的全局状态。每个部分应正确地完成各自的角色，以确保状态管理的清晰和可维护性。
