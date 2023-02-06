- `v-show` ：是渲染组件，然后改变组件的 display 为 block 或 none
   `v-if` ：是渲染或不渲染组件
- 所以对于可以频繁改变条件的场景，就使用 v-show 节省性能，特别是 DOM 结构越复杂收益越大
- 不过它也有劣势，就是 v-show 在一开始的时候，所有分支内部的组件都会渲染，对应的生命周期钩子函数都会执行，而 v-if 只会加载判断条件命中的组件，所以需要根据不同场景使用合适的指令
- 比如下面的用  `v-show`  复用DOM，比  `v-if/v-else`  效果好
- ```html
  <template>
    <div>
      <div v-show="status" class="on">
        <my-components />
      </div>
      <section v-show="!status" class="off">
        <my-components >
      </section>
    </div>
  </template>
  ```
- 原理就是使用 v-if 当条件变化的时候，触发 diff 更新，发现新旧 vnode 不一致，就会移除整个旧的  `vnode` ，再重新创建新的  `vnode` ，然后创建新的  `my-components`  组件，又会经历组件自身初始化， `render` ， `patch`  等过程，而  `v-show`  在条件变化的时候，新旧  `vnode`  是一致的，就不会执行移除创建等一系列流程
-