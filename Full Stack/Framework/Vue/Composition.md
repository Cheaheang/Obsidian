
**7 Composition function**
- **ref**
- **reactive**
- **toRefs**
- **computed**
- **watchEffect**
- **watch** : run callback when value change 
- **lifecycle hooks:** (onMounted, onUpdate, onErrorCapture)
**Fun Fact: ** 
- You should store(number, strings, booleans) for **ref**
- You should store(maps, objects) for **reactive**
- To convert reactive to ref. You need to use : **toRefs**
- Every **callback** is a **function**, but not every **function** is a **callback**

```js
//ref
const counter == ref(0)
//reactive
const counter == reactive({count: 0})
//toRefs
const counter == reactive({count: 0})
const { count } = toRefs(counter)
return { count }
//computed
const count =ref(0)
const time2 = computed (()=> count.value * 2)
```