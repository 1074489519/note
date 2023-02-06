- 为什么要避免同时使用  `v-for`  和  `v-if`
- 在 Vue2 中  `v-for`  优先级更高，所以编译过程中会把列表元素全部遍历生成虚拟 DOM，再来通过 v-if 判断符合条件的才渲染，就会造成性能的浪费，因为我们希望的是不符合条件的虚拟 DOM都不要生成
- 在 Vue3 中  `v-if`  的优先级更高，就意味着当判断条件是 v-for 遍历的列表中的属性的话，v-if 是拿不到的
- 所以在一些需要同时用到的场景，就可以通过计算属性来过滤一下列表，如下
- ```vue
  <template>
      <ul>
        <li v-for="item in activeList" :key="item.id">
          {{ item.title }}
        </li>
      </ul>
  </template>
  <script>
  // Vue2.x
  export default {
      computed: {
        activeList() {
          return this.list.filter( item => {
            return item.isActive
          })
        }
      }
  }
  
  // Vue3
  import { computed } from "vue";
  const activeList = computed(() => {
    return list.filter( item => {
      return item.isActive
    })
  })
  </script>
  ```