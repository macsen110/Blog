## vuex笔记分析 ##


1. Actions与 mutations 区别

    Action 提交的是 mutation，而不是直接变更状态。
    Action 可以包含任意异步操作。
    store.$dispatch(actionName) 触发action
    store.commit(mutationName) 触发mutations,更改store中state状态



2. 组件中使用mutations

    ...mapMutations([
          'increment' // 映射 this.increment() 为 this.$store.commit('increment')
    ]),
    ...mapMutations({
          add: 'increment' // 映射 this.add() 为 this.$store.commit('increment')
    })

3. 组件中使用actions

    ...mapActions([
      'increment' // 映射 this.increment() 为 this.$store.dispatch('increment')
    ]),
    ...mapActions({
      add: 'increment' // 映射 this.add() 为 this.$store.dispatch('increment')
    })

4. 组合Actions

> Action 通常是异步的.store.dispatch 可以处理被触发的action的回调函数返回的Promise，并且store.dispatch仍旧返回Promise

    actions: {
      actionA ({ commit }) {
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            commit('someMutation')
            resolve()
          }, 1000)
        })
      }
    }

    store.dispatch('actionA').then(() => {
      // ...
    })

    actions: {
      // ...
      actionB ({ dispatch, commit }) {
        return dispatch('actionA').then(() => {
          commit('someOtherMutation')
        })
      }
    }
  

    // 假设 getData() 和 getOtherData() 返回的是 Promise

    actions: {
      async actionA ({ commit }) {
        commit('gotData', await getData())
      },
      async actionB ({ dispatch, commit }) {
        await dispatch('actionA') // 等待 actionA 完成
        commit('gotOtherData', await getOtherData())
      }
    }

> 一个 store.dispatch 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。(本质是同步和异步区别)

5. getters

> 有时候我们需要从 store 中的 state 中派生出一些状态, Vuex 允许我们在 store 中定义『getters』（可以认为是 store 的计算属性）

    const store = new Vuex.Store({
      state: {
        todos: [
          { id: 1, text: '...', done: true },
          { id: 2, text: '...', done: false }
        ]
      },
      getters: {
        doneTodos: state => {
          return state.todos.filter(todo => todo.done)
        }
      }
    })

>Getters 会暴露为 store.getters 对象：

    store.getters.doneTodos


> Getters 也可以接受其他 getters 作为第二个参数：

    getters: {
      // ...
      doneTodosCount: (state, getters) => {
        return getters.doneTodos.length
      }
    }
    store.getters.doneTodosCount // -> 1

>我们可以很容易地在任何组件中使用它：

    computed: {
      doneTodosCount () {
        return this.$store.getters.doneTodosCount
      }
    }
  

>mapGetters 辅助函数仅仅是将 store 中的 getters 映射到局部计算属性,也可以别名映射

6. state

> Vuex 使用 单一状态树 —— 是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个『唯一数据源(SSOT)』而存在。这也意味着，每个应用将仅仅包含一个 store 实例。 mapState 辅助函数使用 mapState 辅助函数帮助我们生成计算属性

    import { mapState } from 'vuex'

    export default {
      // ...
      computed: mapState({
        // 箭头函数可使代码更简练
        count: state => state.count,

        // 传字符串参数 'count' 等同于 `state => state.count`
        countAlias: 'count',

        // 为了能够使用 `this` 获取局部状态，必须使用常规函数
        countPlusLocalState (state) {
          return state.count + this.localCount
        },
        'countTest'//当映射的计算属性的名称与 state 的子节点名称相同时
      })
    }


7. Modules

>由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。
Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

    const moduleA = {
      state: { ... },
      mutations: { ... },
      actions: { ... },
      getters: { ... }
    }

    const moduleB = {
      state: { ... },
      mutations: { ... },
      actions: { ... }
    }

    const store = new Vuex.Store({
      modules: {
        a: moduleA,
        b: moduleB
      }
    })

    store.state.a // -> moduleA 的状态
    store.state.b // -> moduleB 的状态


对于模块内部的 mutation 和 getter，接收的第一个参数是模块的局部状态对象。

    const moduleA = {
      state: { count: 0 },
      mutations: {
        increment (state) {
          // 这里的 `state` 对象是模块的局部状态
          state.count++
        }
      },

      getters: {
        doubleCount (state) {
          return state.count * 2
        }
      }
    }


同样，对于模块内部的 action，局部状态通过 context.state 暴露出来， 根节点状态则为 context.rootState：

    const moduleA = {
      // ...
      actions: {
        incrementIfOddOnRootSum ({ state, commit, rootState }) {
          if ((state.count + rootState.count) % 2 === 1) {
            commit('increment')
          }
        }
      }
    }


对于模块内部的 getter，根节点状态会作为第三个参数暴露出来：

    const moduleA = {
      // ...
      getters: {
        sumWithRootCount (state, getters, rootState) {
          return state.count + rootState.count
        }
      }
    }


具体请看  http://vuex.vuejs.org/zh-cn/modules.html