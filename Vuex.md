# vuex 原理

### 常见关键字字
```javascript
Store({
  state: {},
  mutations: {},
  actions: {},
  getter: {},
  modules: {}
})
```

### 实现获取state 可以通过`this.$store.state`访问state里面的值
```javascript
class Store{
    constructor(options) {
        this.vm = new Vue({
            data:{
                state:options.state
            }
        })
    }
    // 新增代码
    get state(){
        return this.vm.state
    }
}
```

### 实现getter  可以通过`this.$store.getters` 访问getters里面的值
```javascript
class Store{
    constructor(options) {
        this.vm = new Vue({
            data:{
                state:options.state
            }
        })

        // 新增代码
        let getters = options.getter || {}
        this.getters = {}
        Object.keys(getters).forEach(getterName=>{
            Object.defineProperty(this.getters, getterName,{
                get:()=>{
                    return getters[getterName](this.state)
                }
            })
        })
    }
    get state(){
        return this.vm.state
    }
}
```

### 实现mutations/commit 收集mutations函数并且给回调的参数 可以通过`this.$store.commit`函数改变state里面的值
```javascript
class Store{
    constructor(options) {
        this.vm = new Vue({
            data:{
                state:options.state
            }
        })


        let getters = options.getter || {}
        this.getters = {}
        Object.keys(getters).forEach(getterName=>{
            Object.defineProperty(this.getters, getterName,{
                get:()=>{
                    return getters[getterName](this.state)
                }
            })
        })

        //新增代码
        let mutations = options.mutations || {}
        this.mutations = {}
        Object.keys(mutations).forEach(mutationName=>{
            this.mutations[mutationName] = (arg)=> {
                mutations[mutationName](this.state,arg)
            }
        })

    }

    //新增代码
    commit(method,arg){
        this.mutations[method](arg)
    }

    get state(){
        return this.vm.state
    }
}
```

### 实现actions/dispatch

```javascript
class Store{
    constructor(options) {
        this.vm = new Vue({
            data:{
                state:options.state
            }
        })


        let getters = options.getter || {}
        this.getters = {}
        Object.keys(getters).forEach(getterName=>{
            Object.defineProperty(this.getters, getterName,{
                get:()=>{
                    return getters[getterName](this.state)
                }
            })
        })


        let mutations = options.mutations || {}
        this.mutations = {}
        Object.keys(mutations).forEach(mutationName=>{
            this.mutations[mutationName] = (arg)=> {
                mutations[mutationName](this.state,arg)
            }
        })

        //新增代码
        let actions = options.actions
        this.actions = {}
        Object.keys(actions).forEach(actionName=>{
            this.actions[actionName] = (arg)=>{
                actions[actionName](this, arg)
            }
        })
    }


    // 新增代码
    dispatch (method,arg){
        this.actions[method](arg)
    }
    
    commit = (method,arg) =>{
        this.mutations[method](arg)
    }

    get state(){
        return this.vm.state
    }
}
```
