

## 一、vue3新特性

### 性能提升

- 打包大小减少 41%
- 初次渲染快 55%，更新快133%
- 内存使用减少 54%

### composition API

- ref 和 reactive
- computed 和 watch
- 新的生命周期函数
- 自定义函数 - Hooks函数

### 其他新增特性

- Teleport - 瞬移组件的位置
- Suspense - 异步加载组件的新福音
- 全局 API 的修改和优化
- 更多的实验性特征
- 更好的 Typescript 支持

### vue2 遇到的难题

- **随着功能的增长，复杂组件的代码变得难以维护**

例如：

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue302.4oioggdsl0c0.webp" alt="image-20210721104224965" style="zoom:60%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue03.38hp3erbfvq0.webp" alt="image-20210721105445672" style="zoom:67%;" />

随着复杂度提升，带来的问题

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue04.45n9zxzbsfo0.webp" alt="image-20210721110357840" style="zoom:50%;" />

vue2解决方式==Mixin==

<center><img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue305.3kayo6uiry00.webp" alt="image-20210721110537880" style="zoom:67%;" /><img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue306.f6l93u44kyg.webp" alt="image-20210721110652071" style="zoom:50%;" /></center>

## 二、使用 vue-cli 配置 vue3

安装：

```bash
npm install -g @vue/cli
# OR
yarn global add @vue/cli

# 查看版本
vue --version
```

创建一个项目：

```bash
vue create my-project
# OR
vue ui
```

```bash
vue create my-project

> Manully select features

(*) Choose Vue version
(*) Babel
(*) TypeScript
( ) Progressive Web App (PWA) Support
(*) Router
(*) Vuex
( ) CSS Pre-processors
(*) Linter / Formatter
( ) Unit Testing
( ) E2E Testing

> 3.x

? Use class-style component syntax? (y/N) n

? Use Babel alongside TypeScript(required for modern mode，auto-detected polyfills, transpiling JSX)? (Y/n) n

? Pick a linter / formatter config:
> ESLint with error prevention only

> Lint on save

>In dedicated config files

? Save this as a preset for future projects? (y/N)n #保存下来一键安装
```

## 三、ref 的妙用

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ count }}</h1>
    <h2>{{ double }}</h2>
    <button @click="increate">赞+1</button>
  </div>
</template>

<script lang="ts">
import { ref, computed } from 'vue'
export default {
  name: 'Home',
  setup() {
    //无法访问this
    const count = ref(0)
    const double = computed(() => {
      return count.value * 2
    })
    const increate = () => {
      count.value++
    }
    return {
      count,
      increate,
      double,
    }
  },
}
</script>
```

## 四、reactive

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ data.count }}</h1>
    <h2>{{ data.double }}</h2>
    <button @click="data.increate">赞+1</button>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive } from 'vue'
interface DataProps {
  count: number
  double: number
  increate: () => void
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
    })
    return {
      data,
    }
  },
}
</script>

```

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ count }}</h1>
    <h2>{{ double }}</h2>
    <button @click="increate">赞+1</button>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs } from 'vue'
// toRefs 是一个函数，它接收一个reactive对象作为参数，返回一个普通的对象，但这个对象的每一项都变成ref类型的对象
interface DataProps {
  count: number
  double: number
  increate: () => void
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
    })
    const refData = toRefs(data)
    // data.count 是number对象不是响应式对象
    // refData.count 此时变成响应式对象
    return {
      ...refData,
    }
  },
}
</script>

```

## 五、vue3响应式对象

### proxy

proxy 队形用于定义基本操作的自定义行为（如属性查找、赋值、枚举、函数调用等）

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ count }}</h1>
    <!-- <h1>{{ data.count }}</h1> -->
    <h2>{{ double }}</h2>
    <!-- <h2>{{ data.double }}</h2> -->
    <ul>
      <li v-for="item in numbers" :key="item">{{ item }}</li>
    </ul>
    <h3>{{ person.name }}</h3>
    <button @click="increate">赞+1</button>
    <!-- <button @click="data.increate">赞+1</button> -->
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs } from 'vue'
// toRefs 是一个函数，它接收一个reactive对象作为参数，返回一个普通的对象，但这个对象的每一项都变成ref类型的对象
interface DataProps {
  count: number
  double: number
  increate: () => void
  numbers: number[]
  person: { name?: string }
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
      numbers: [0, 1, 2],
      person: {},
    })
    data.numbers[0] = 5
    data.person.name = 'viking'
    const refData = toRefs(data)
    return {
      ...refData,
      // data,
    }
  },
}
</script>
```

## 六、生命周期

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue307.1vjl2hq1vfts.webp" alt="image-20210723164601951" style="zoom:67%;" />

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ count }}</h1>
    <!-- <h1>{{ data.count }}</h1> -->
    <h2>{{ double }}</h2>
    <!-- <h2>{{ data.double }}</h2> -->
    <ul>
      <li v-for="item in numbers" :key="item">{{ item }}</li>
    </ul>
    <h3>{{ person.name }}</h3>
    <button @click="increate">赞+1</button>
    <!-- <button @click="data.increate">赞+1</button> -->
  </div>
</template>

<script lang="ts">
import {
  ref,
  computed,
  reactive,
  toRefs,
  onMounted,
  onUpdated,
  onRenderTracked,
} from 'vue'
// toRefs 是一个函数，它接收一个reactive对象作为参数，返回一个普通的对象，但这个对象的每一项都变成ref类型的对象
interface DataProps {
  count: number
  double: number
  increate: () => void
  numbers: number[]
  person: { name?: string }
}
export default {
  name: 'Home',
  setup() {
    onRenderTracked((event) => {
      console.log(event)
    })
    onMounted(() => {
      console.log('mounted')
    })
    onUpdated(() => {
      console.log('updata')
    })
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
      numbers: [0, 1, 2],
      person: {},
    })
    data.numbers[0] = 5
    data.person.name = 'viking'
    const refData = toRefs(data)
    return {
      ...refData,
      // data,
    }
  },
}
</script>


```

## 七、侦测变化

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ count }}</h1>
    <h2>{{ double }}</h2>
    <h3>{{ greetings }}</h3>
    <button @click="increate">赞+1</button>
    <button @click="updateGreeting">Update</button>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs, watch } from 'vue'
interface DataProps {
  count: number
  double: number
  increate: () => void
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
    })
    const greetings = ref('')
    const updateGreeting = () => {
      greetings.value += 'Hello! '
    }
    // watch(greetings, (newValue,oldValue) => {
    //   console.log('newValue',newValue)
    //   console.log('oldValue',oldValue)
    //   document.title = `updated${greetings.value}`
    // })
    // watch([greetings, data], (newValue, oldValue) => {
    watch([greetings, () => data.count], (newValue, oldValue) => {
      console.log('newValue', newValue)
      console.log('oldValue', oldValue)
      document.title = `updated${greetings.value}`
    })
    const refData = toRefs(data)
    return {
      ...refData,
      greetings,
      updateGreeting,
    }
  },
}
</script>
```

## 八、vue3模块化

### 鼠标跟踪器

初始化：

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ count }}</h1>
    <h2>{{ double }}</h2>
    <h3>{{ greetings }}</h3>
    <h3>X:{{ x }},Y:{{ y }}</h3>
    <button @click="increate">赞+1</button>
    <button @click="updateGreeting">Update</button>
  </div>
</template>

<script lang="ts">
import {
  ref,
  computed,
  reactive,
  toRefs,
  watch,
  onMounted,
  onUnmounted,
} from 'vue'
interface DataProps {
  count: number
  double: number
  increate: () => void
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
    })
    const greetings = ref('')
    const updateGreeting = () => {
      greetings.value += 'Hello! '
    }
    const x = ref(0)
    const y = ref(0)
    const updateMouse = (e: MouseEvent) => {
      x.value = e.pageX
      y.value = e.pageY
    }
    onMounted(() => {
      document.addEventListener('click', updateMouse)
    })
    onUnmounted(() => {
      document.removeEventListener('click', updateMouse)
    })
    watch([greetings, () => data.count], (newValue, oldValue) => {
      console.log('newValue', newValue)
      console.log('oldValue', oldValue)
      document.title = `updated${greetings.value}`
    })
    const refData = toRefs(data)
    return {
      ...refData,
      greetings,
      updateGreeting,
      x,
      y,
    }
  },
}
</script>
```

ts公共方法抽取：

新建  @/src/==hooks/useMousePosition.ts==

```typescript
import { ref, onMounted, onUnmounted } from 'vue'
function useMousePosition() {
  const x = ref(0)
  const y = ref(0)
  const updateMouse = (e: MouseEvent) => {
    x.value = e.pageX
    y.value = e.pageY
  }
  onMounted(() => {
    document.addEventListener('click', updateMouse)
  })
  onUnmounted(() => {
    document.removeEventListener('click', updateMouse)
  })
  return { x, y }
}
export default useMousePosition
```

引入：

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ count }}</h1>
    <h2>{{ double }}</h2>
    <h3>{{ greetings }}</h3>
    <h3>X:{{ x }},Y:{{ y }}</h3>
    <button @click="increate">赞+1</button>
    <button @click="updateGreeting">Update</button>
  </div>
</template>

<script lang="ts">
import {
  ref,
  computed,
  reactive,
  toRefs,
  watch,
  onMounted,
  onUnmounted,
} from 'vue'
import useMousePosition from '../hooks/useMousePosition'
interface DataProps {
  count: number
  double: number
  increate: () => void
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
    })
    // 1、与mixin比较清楚的知道想x，y的来源
    // 2、可以给x,y重新设置变量名称，避免命名冲突
    // 3、业务逻辑脱离组件而存在
    const { x, y } = useMousePosition()
    const greetings = ref('')
    const updateGreeting = () => {
      greetings.value += 'Hello! '
    }
    watch([greetings, () => data.count], (newValue, oldValue) => {
      console.log('newValue', newValue)
      console.log('oldValue', oldValue)
      document.title = `updated${greetings.value}`
    })
    const refData = toRefs(data)
    return {
      ...refData,
      greetings,
      updateGreeting,
      x,
      y,
    }
  },
}
</script>

```

### 模块化难度上升 - 加载逻辑

ts公共方法抽取：

新建  @/src/==hooks/useURLLoader.ts==

```typescript
import { ref } from 'vue'
import axios from 'axios'

function useURLLoader(url: string) {
  const result = ref(null)
  const loading = ref(true)
  const loaded = ref(false)
  const error = ref(null)
  axios
    .get(url)
    .then((rawData) => {
      loading.value = false
      loaded.value = true
      result.value = rawData.data
    })
    .catch((e) => {
      error.value = e
      loading.value = false
    })
  return {
    result,
    error,
    loading,
    loaded,
  }
}
export default useURLLoader
```

引入：

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>{{ count }}</h1>
    <h2>{{ double }}</h2>
    <h3>{{ greetings }}</h3>
    <h3>X:{{ x }},Y:{{ y }}</h3>
    <h3 v-if="loading">Loading!...</h3>
    <img v-if="loaded" :src="result.message" alt="" />
    <button @click="increate">赞+1</button>
    <button @click="updateGreeting">Update</button>
  </div>
</template>

<script lang="ts">
import {
  ref,
  computed,
  reactive,
  toRefs,
  watch,
  onMounted,
  onUnmounted,
} from 'vue'
import useMousePosition from '../hooks/useMousePosition'
import useURLLoader from '../hooks/useURLLoader'
interface DataProps {
  count: number
  double: number
  increate: () => void
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
    })
    const { x, y } = useMousePosition()
    const { result, error, loading, loaded } = useURLLoader(
      'https://dog.ceo/api/breeds/image/random'
    )
    const greetings = ref('')
    const updateGreeting = () => {
      greetings.value += 'Hello! '
    }
    watch([greetings, () => data.count], (newValue, oldValue) => {
      console.log('newValue', newValue)
      console.log('oldValue', oldValue)
      document.title = `updated${greetings.value}`
    })
    const refData = toRefs(data)
    return {
      ...refData,
      greetings,
      updateGreeting,
      x,
      y,
      result,
      error,
      loading,
      loaded,
    }
  },
}
</script>
```

### 模块化结合Typescript - 泛型改造

获取请求结果result

```typescript
    const { result, error, loading, loaded } = useURLLoader(
      'https://dog.ceo/api/breeds/image/random'
    )
    watch(result, () => {
      console.log(result.value)
      console.log(result.value.message) //result类型为null，取 result.value.message 会报错
    })
```

使用泛型

```typescript
import { ref } from 'vue'
import axios from 'axios'

function useURLLoader<T>(url: string) {
  const result = ref<T | null>(null)
  const loading = ref(true)
  const loaded = ref(false)
  const error = ref(null)
  axios
    .get(url)
    .then((rawData) => {
      loading.value = false
      loaded.value = true
      result.value = rawData.data
    })
    .catch((e) => {
      error.value = e
      loading.value = false
    })
  return {
    result,
    error,
    loading,
    loaded,
  }
}
export default useURLLoader
```

引入：

```vue
<script lang="ts">
import {
  ref,
  computed,
  reactive,
  toRefs,
  watch,
} from 'vue'
import useURLLoader from '../hooks/useURLLoader'
interface DataProps {
  count: number
  double: number
  increate: () => void
}
interface DogResult {
  message: string
  status: string
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
    })
    const { result, error, loading, loaded } = useURLLoader<DogResult>(
      'https://dog.ceo/api/breeds/image/random'
    )
    watch(result, () => {
      if (result.value) {
        console.log(result.value)
        console.log(result.value.message) // typescript 自动补全
      }
    })
    const refData = toRefs(data)
    return {
      ...refData,
      result,
      error,
      loading,
      loaded,
    }
  },
}
```

泛型改造：

接口类型

![image-20210726113125315](https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue327.heg0aejkvmo.webp)

```vue
<template>
  <div class="home">
    <h1>{{ count }}</h1>
    <h2>{{ double }}</h2>
    <h3 v-if="loading">Loading!...</h3>
    <img v-if="loaded" :src="result[0].url" alt="" />
    <button @click="increate">赞+1</button>
  </div>
</template>
<script lang="ts">
import {
  ref,
  computed,
  reactive,
  toRefs,
  watch,
} from 'vue'
import useURLLoader from '../hooks/useURLLoader'
interface DataProps {
  count: number
  double: number
  increate: () => void
}
interface DogResult {
  message: string
  status: string
}
interface catResult {
  id: string
  url: string
  width: number
  height: number
}
export default {
  name: 'Home',
  setup() {
    const data: DataProps = reactive({
      count: 0,
      increate: () => {
        data.count++
      },
      double: computed(() => data.count * 2),
    })
    // const { result, error, loading, loaded } = useURLLoader<DogResult>(
    //   'https://dog.ceo/api/breeds/image/random'
    // )
    const { result, error, loading, loaded } = useURLLoader<catResult[]>(
      'https://api.thecatapi.com/v1/images/search?limit=1&size=full&sub_id=demo-8d935f'
    )
    watch(result, () => {
      if (result.value) {
        console.log(result.value)
        console.log(result.value.message) // typescript 自动补全
      }
    })
    const refData = toRefs(data)
    return {
      ...refData,
      result,
      error,
      loading,
      loaded,
    }
  },
}
```

Typescript 对 vue3 的加持

vue2遇到的难题

- vue2 对于typescript 的支持非常的有限

```vue
<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
// 对于ts来说使用了非常好用的 defineComponent 方法，
// 以后使用的组件都用 defineComponent 它来包裹
// 它让传入的对象获得非常多的类型
defineComponent({
  name: 'Typescript',
  props: {
    msg: {
      require: true,
      type: String,
    },
  },
  // setup 方法的两个参数
  // props 是响应式对象
  setup(props, context) {
    props.msg
    //context提供了vue2最常用的3个属性 this对象
    context.emit
    context.slots
    context.attrs
  },
})
export default {}
</script>

<style></style>
```

## 九、Teleport - 瞬间移动

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue308.2hjc19rh8t60.webp" alt="image-20210727104049132" style="zoom:50%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue309.6xq2q1fxyg00.webp" alt="image-20210727104314122" style="zoom:50%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue309.6xq2q1fxyg00.webp" alt="image-20210727104336739" style="zoom:50%;" />

### 初步

创建组件：

```vue
<template>
    <!-- 使用 teleport 标签，to 去绑定标签 -->
  <teleport to="#model">
    <div id="center">
      <h2>this is a teleport</h2>
    </div>
  </teleport>
</template>

<script lang="ts">
export default {}
</script>

<style>
#center {
  width: 200px;
  height: 200px;
  border: 2px solid black;
  background: #fff;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>
```

引用组件：

```vue
<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png" />
    <model></model>
  </div>
</template>

<script lang="ts">
import Model from './model.vue'
export default {
  name: 'Teleport',
  components: {
    Model,
  },
  // setup() {},
}
</script>

<style></style>
```

绑定标签：

index.html

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <div id="model"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

显示结果：

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue0311.3l9meoivyza0.webp" alt="image-20210727112046187" style="zoom:80%;" />

### emit、props

组件：

```vue
<template>
  <!-- 使用 teleport 标签，to 去绑定标签 -->
  <teleport to="#model">
    <div id="center" v-if="isOpen">
      <h2><slot>this is a teleport</slot></h2>
      <button @click="buttonClick">关闭</button>
    </div>
  </teleport>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  props: {
    isOpen: Boolean,
  },
  emits: {
    'close-model': null,
  },
  setup(props, context) {
    // context.emit('close-model', { type: 'hello' })
    const buttonClick = () => {
      context.emit('close-model')
    }
    return { buttonClick }
  },
})
</script>

<style>
#center {
  width: 200px;
  height: 200px;
  border: 2px solid black;
  background: #fff;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>
```

引用：

```vue
<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png" />
    <button @click="openModel">打开</button>
    <model :isOpen="modelIsOpen" @close-model="onModelClose">My model !!</model>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue'
import Model from './model.vue'
export default {
  name: 'Teleport',
  components: {
    Model,
  },
  setup() {
    const modelIsOpen = ref(false)
    const openModel = () => {
      modelIsOpen.value = true
    }
    const onModelClose = () => {
      modelIsOpen.value = false
    }
    return {
      modelIsOpen,
      openModel,
      onModelClose,
    }
  },
}
</script>

<style></style>
```

绑定：

```html
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <div id="model"></div>
    <!-- built files will be auto injected -->
  </body>
```

## 十、Suspense - 异步请求

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue0312.4k9nzixsijy0.webp" alt="image-20210727114603732" style="zoom:50%;" />

### 初识suspense 

创建AsyncShow.vue组件

```vue
<template>
  <div>
    {{ result }}
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  setup() {
    return new Promise((resolve) => {
      setTimeout(() => {
        return resolve({
          //使用suspense直接返回result即可
          result: 41,
        })
      }, 3000)
    })
  },
})
</script>
<style></style>

```

引入组件

```vue
<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png" />
    <button @click="openModel">打开</button>
    <model :isOpen="modelIsOpen" @close-model="onModelClose">My model !!</model>
    <Suspense>
      <!-- 展示result内容 -->
      <template #default>
        <async-show></async-show>
      </template>
      <!-- #fallback 展示没有result之间展示的内容 -->
      <template #fallback>
        <h1>loading...</h1>
      </template>
    </Suspense>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue'
import Model from './model.vue'
import AsyncShow from './AsyncShow.vue'
export default {
  name: 'Teleport',
  components: {
    Model,
    AsyncShow,
  },
  setup() {
    const modelIsOpen = ref(false)
    const openModel = () => {
      modelIsOpen.value = true
    }
    const onModelClose = () => {
      modelIsOpen.value = false
    }
    return {
      modelIsOpen,
      openModel,
      onModelClose,
    }
  },
}
</script>

<style></style>

```

### 加深使用suspense

创建DogShow.vue组件

```vue
<template>
  <img :src="result && result.message" alt="" />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios'
export default defineComponent({
  async setup() {
    const rawData = await axios.get('https://dog.ceo/api/breeds/image/random')
    console.log(rawData.data)
    return {
      result: rawData.data,
    }
  },
})
</script>

<style></style>
```

引入DowShow.vue组件

```vue
<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png" />
    <button @click="openModel">打开</button>
    <model :isOpen="modelIsOpen" @close-model="onModelClose">My model !!</model>
    <Suspense>
      <!-- 展示result内容 -->
      <!-- 等两个组件都加载完毕后，一起显示 -->
      <template #default>
        <async-show></async-show>
        <dog-show></dog-show>
      </template>
      <!-- #fallback 展示没有result之间展示的内容 -->
      <template #fallback>
        <h1>loading...</h1>
      </template>
    </Suspense>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue'
import Model from './model.vue'
import AsyncShow from './AsyncShow.vue'
import DogShow from './DogShow.vue'
export default {
  name: 'Teleport',
  components: {
    Model,
    AsyncShow,
    DogShow,
  },
  setup() {
    const modelIsOpen = ref(false)
    const openModel = () => {
      modelIsOpen.value = true
    }
    const onModelClose = () => {
      modelIsOpen.value = false
    }
    return {
      modelIsOpen,
      openModel,
      onModelClose,
    }
  },
}
</script>

<style></style>

```

### 抓取组件错误

onErrorCaptured

```vue
<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png" />
    <button @click="openModel">打开</button>
    <model :isOpen="modelIsOpen" @close-model="onModelClose">My model !!</model>
    <p>{{ error }}</p>
    <Suspense>
      <!-- 展示result内容 -->
      <template #default>
        <async-show></async-show>
        <DogShow></DogShow>
        <!-- <dog-show></dog-show> -->
      </template>
      <!-- #fallback 展示没有result之间展示的内容 -->
      <template #fallback>
        <h1>loading...</h1>
      </template>
    </Suspense>
  </div>
</template>

<script lang="ts">
import { ref, onErrorCaptured } from 'vue'
import Model from './model.vue'
import AsyncShow from './AsyncShow.vue'
import DogShow from './DogShow.vue'
export default {
  name: 'Teleport',
  components: {
    Model,
    AsyncShow,
    DogShow,
  },
  setup() {
    const error = ref(null)
    onErrorCaptured((err: any) => {
      error.value = err
      return true // 这个钩子函数最后返回一个boolean值
    })
    const modelIsOpen = ref(false)
    const openModel = () => {
      modelIsOpen.value = true
    }
    const onModelClose = () => {
      modelIsOpen.value = false
    }
    return {
      modelIsOpen,
      openModel,
      onModelClose,
      error,
    }
  },
}
</script>

<style></style>

```

## 十一、项目实践

### 11.1文件结构和代码规范

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue313.jsazn3nd218.webp" alt="image-20210823104426339" style="zoom:67%;" />

- 组件文件名开头大写
- 文件夹、ts文件、图片文件命名开头小写+驼峰式
- 代码规范 [eslint-plugin-vue](https://eslint.vuejs.org/rules/)、[Javascript Standard](https://standardjs.com/) 或 https://github.com/standard/standard/blob/master/docs/README-zhcn.md

### 11.2 UI库

- boostrap
- tailwindcss

安装：

```shell
npm install bootstrap@next --save
```

### 11.3 开发流程

- 将 UI 划分出组件的层级
- 创建应用的静态版本

### 11.4 组件

#### 1. PropType 强制转换

使用`vue3.0` 对 `props` 进行复杂类型验证的时候，可以直接用 `PropType` 进行强制转换：

无法直接使用类型断言

```typescript
  props: {
    list: {
      type: Array as ColomnProps[]
    }
  },
```

可以直接用 `PropType` 进行强制转换

```vue
<script lang='ts'>
import {defineComponent,PropType} from 'vue'
export interface ColumnProps{
	id: string;
	title: string;
  avatar: string;
	description:string;
}
export default definecomponent({
	name : " ColumnList ' ,
	props:{
		list:{
			type : Array as PropType<columnProps[]>,
      required : true
		}
	}
})
</ script>

```

#### 2. 引入组件和方法

```typescript
import ColumnList,{ColomnProps} from './components/ColumnList.vue'
const testData: ColomnProps[] = [
  {
    id: 1,
    title: 'test1的专栏',
    description:
      '这是的test1专栏，有一段非常有意思的简介，可以更新一下欧, 这是的test1专栏，有一段非常有意思的简介，可以更新一下欧',
    avatar:
      'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_150,w_150'
  }]
```

#### 3. 计算重组 List 属性

使用 computed 和 map 计算重组 list 属性

```vue
<template>
  <ul class="row">
    <li v-for="column in columnList" :key="column.id" class="col-4 mb-4">
      <div class="card h-100 shadow-sm">
        <div class="card-body text-center">
          <img
            :src="column.avatar"
            class="rounded-circle border border-light w-25 my-3"
            :alt="column.title"
          />
          <h5 class="card-title">{{ column.title }}</h5>
          <p class="card-text text-left">
            {{ column.description }}
          </p>
          <a href="#" class="btn btn-outline-primary">进入专栏</a>
        </div>
      </div>
    </li>
  </ul>
</template>

<script lang="ts">
import { computed, defineComponent, PropType } from 'vue'
export interface ColomnProps {
  id: number
  title: string
  avatar?: string
  description: string
}
export default defineComponent({
  name: 'ColumnList',
  props: {
    list: {
      type: Array as PropType<ColomnProps[]>
    }
  },
  setup(props) {
    const columnList = computed(() => {
      return props.list?.map((column) => {
        if (!column.avatar) {
          column.avatar = require('@/assets/images/avatar.jpg')
        }
        return column
      })
    })
    return {
      columnList
      // list
    }
  }
})
</script>
<style lang="less" scoped></style>
```

#### 4. @click.prevent 防止默认行为

\<a>元素的点击事件 @click 上添加 增加 prevent 防止默认行为

```vue
<!-- @click.prevent 增加 prevent 防止默认行为  -->
<a href="#" class="btn btn-outline-light my-2 dropdown-toggle" @click.prevent="toggleOpen">
	{{ title }}
</a>
```

#### 5. 封装下拉菜单，支持 disabled 等多属性

封装 DropdownItem.vue 组件

==通配符 * 设置样式==，:class对象支持多属性。

```vue
<template>
  <li class="dropdown-option dropdown-item" :class="{'is-disabled':disabled}">
{{disabled}}
    <slot></slot>
  </li>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  props: {
    disabled: {
      type: Boolean,
      default: false
    }
  },
  setup() {
    return {}
  }
})
</script>
<style>
.dropdown-option.is-disabled * {
  color: #6c757d;
  pointer-events: none;
  background-color: transparent;
}
</style>
```

Dropdown.vue 组件支持 \<slot>插槽

```vue
<template>
  <div class="dropdown" ref="dropdownRef">
    <!-- @click.prevent 增加 prevent 防止默认行为  -->
    <a href="#" class="btn btn-outline-light my-2 dropdown-toggle" @click.prevent="toggleOpen">
      {{ title }}
    </a>
    <ul class="dropdown-menu" :style="{ display: 'block' }" v-if="isOpen">
    <slot></slot>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
export default defineComponent({
  name: 'Dropdown',
  props: {
    title: {
      type: String,
      required: true
    }
  },
  setup() {
    const isOpen = ref(false)
    const toggleOpen = () => {
      isOpen.value = !isOpen.value
    }
    return {
      isOpen,
      toggleOpen
    }
  }
})
</script>
<style lang="less" scoped></style>
```

顶部组件引用 GlobalHeader.vue 

```vue
<template>
  <div>
    <nav
      class="navbar navbar-dark bg-primary justify-content-between mb-4 px-4"
    >
      <a class="navbar-brand" href="#">者也专栏</a>
      <ul v-if="!user.isLogin">
        <li class="list-inline-item">
          <router-link to="/login" class="btn btn-outline-light my-2"
            >登陆</router-link
          >
        </li>
        <li class="list-inline-item">
          <router-link to="/signup" class="btn btn-outline-light my-2"
            >注册</router-link
          >
        </li>
      </ul>
      <ul v-else class="list-inline mb-0">
        <li class="list-inline-item">
          <dropdown :title="`你好 ${user.name}`">
            <!-- -------- 菜单下拉组件引入 -----------  -->
            <dropdown-item><a href="#">新建文章</a></dropdown-item>
            <dropdown-item disabled><a href="#">编辑资料</a></dropdown-item>
            <dropdown-item><a href="#">退出登录</a></dropdown-item>
          </dropdown>
        </li>
      </ul>
    </nav>
  </div>
</template>
<script lang="ts">
import { defineComponent, PropType } from 'vue'
import Dropdown from './Dropdown.vue'
import DropdownItem from './DropdownItem.vue'
export interface UserProps {
  isLogin: boolean;
  name?: string;
  id?: number;
}
export default defineComponent({
  name: 'GlobalHeader',
  props: {
    user: {
      type: Object as PropType<UserProps>,
      required: true
    }
  },
  components: {
    Dropdown,
    DropdownItem
  },
  setup() {
    return {
      Dropdown,
      DropdownItem
    }
  }
})
</script>
```

#### 6. Dropdown 组件点击外部区域自动隐藏

- 注册初始化触发事件，并销毁事件onMounted、onUnmounted
- 定义ref 元素两种类型 <null | HTMLElement>
- 使用 contains 判断是否包含该元素

```vue
<template>
  <!-- 1. ref 绑定元素 -->
  <div class="dropdown" ref="dropdownRef">
    <!-- @click.prevent 增加 prevent 防止默认行为  -->
    <a
      href="#"
      class="btn btn-outline-light my-2 dropdown-toggle"
      @click.prevent="toggleOpen"
    >
      {{ title }}
    </a>
    <ul class="dropdown-menu" :style="{ display: 'block' }" v-if="isOpen">
      <slot></slot>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted } from 'vue'
export default defineComponent({
  name: 'Dropdown',
  props: {
    title: {
      type: String,
      required: true
    }
  },
  setup() {
    const isOpen = ref(false)
    // 2. 定义 dropdownRef 两种类型 <null | HTMLElement>
    const dropdownRef = ref<null | HTMLElement>(null)
    const toggleOpen = () => {
      isOpen.value = !isOpen.value
    }
    // 3. 点击事件
    const handler = (e: MouseEvent) => {
      console.log(dropdownRef.value)
      if (dropdownRef.value) {
        // 使用 contains 判断是否包含该元素
        // as 类型断言e.target类型
        if (
          !dropdownRef.value.contains(e.target as HTMLElement) &&
          isOpen.value
        ) {
          isOpen.value = false
        }
      }
    }
    // 4. 注册事件
    onMounted(() => {
      document.addEventListener('click', handler)
    })
    // 5. 销毁事件
    onUnmounted(() => {
      document.removeEventListener('click', handler)
    })
    return {
      isOpen,
      toggleOpen,
      dropdownRef
    }
  }
})
</script>
<style lang="less" scoped></style>
```

#### 7. 封装公共函数

在 hooks 文件夹封装 [判断元素触发事件是否是除自己以外元素] 的公共函数，新建useClickOutside.ts文件

```typescript
import { ref, onMounted, onUnmounted, Ref } from 'vue'
const useClickOutside = (elementRef: Ref<null | HTMLElement>) => {
  const isClickOutside = ref(false)
  const handler = (e: MouseEvent) => {
    if (elementRef.value) {
      if (elementRef.value.contains(e.target as HTMLElement)) {
        isClickOutside.value = false
      } else {
        isClickOutside.value = true
      }
    }
  }
  onMounted(() => {
    document.addEventListener('click', handler)
  })
  onUnmounted(() => {
    document.removeEventListener('click', handler)
  })
  return isClickOutside
}

export default useClickOutside
```

引入

```vue
<template>
  <!-- 1. ref 绑定元素 -->
  <div class="dropdown" ref="dropdownRef">
    <!-- @click.prevent 增加 prevent 防止默认行为  -->
    <a
      href="#"
      class="btn btn-outline-light my-2 dropdown-toggle"
      @click.prevent="toggleOpen"
    >
      {{ title }}
    </a>
    <ul class="dropdown-menu" :style="{ display: 'block' }" v-if="isOpen">
      <slot></slot>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue'
// 1.引入文件
import useClickOutside from '../hooks/useClickOutside'
export default defineComponent({
  name: 'Dropdown',
  props: {
    title: {
      type: String,
      required: true
    }
  },
  setup() {
    const isOpen = ref(false)
    // 2. 定义 dropdownRef 两种类型 <null | HTMLElement>
    const dropdownRef = ref<null | HTMLElement>(null)
    const toggleOpen = () => {
      isOpen.value = !isOpen.value
    }
    // 2.引入函数，isClickOutside是一个ref变量
    const isClickOutside = useClickOutside(dropdownRef)
    // 3. 监听变化，注意：这里监听的是isClickOutside,而不是isClickOutside.value
    watch(isClickOutside, () => {
      if (isOpen.value && isClickOutside.value) {
        isOpen.value = false
      }
    })
    return {
      isOpen,
      toggleOpen,
      dropdownRef
    }
  }
})
</script>
<style lang="less" scoped></style>

```

#### 8. input 校验及 v-model 双向绑定封装

##### v-model原理

```vue
// vue2原生组件
<input v-model= "val">
<input :value="val" @input="val = $event.target.value">

// vue2 自定义组件
<my-component v-model='val'/>
<my-component :value= 'val' @input='val =arguments[0]'/>
//非同寻常的表单元素
<input type="checkbox" : checked= 'val' @change="">
// vue3 compile 以后的结果
<my-component v-model= 'foo'/>
h( Comp,{
  modelValue: foo,
  'onUpdate modelValue' : value =>( foo = value)
})

```

例如，check复选框

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue0314.2vwkd6m9p1k0.webp" alt="image-20210928175903469" style="zoom:67%;" />

但只能绑定一个model对象，如果需双向绑定多个值，在现有的系统下很难完成

新建 ValidateInput.vue 文件

```vue
<template>
  <div class="validate-input-container pb-3">
    <input
      type="text"
      class="form-control"
      :class="{ 'is-invalid': inputRef.error }"
      :value="inputRef.val"
      @blur="validateInput"
      @input="updateValue"
    />
    <!-- :value 绑定值， @input 触发value联动 -->
    <span v-if="inputRef.error" class="invalid-feedback">
      {{ inputRef.message }}
    </span>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, PropType } from 'vue'
const emailReg = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
interface RuleProp {
  type: 'required' | 'email';
  message: string;
}
export type RulesProp = RuleProp[];
export default defineComponent({
  props: {
    rules: Array as PropType<RulesProp>,
    modelValue: String
  },
  setup(props, context) {
    const inputRef = reactive({
      val: props.modelValue || '',
      error: false,
      message: ''
    })
    // 双向绑定更新
    const updateValue = (e: KeyboardEvent) => {
      const targetValue = (e.target as HTMLInputElement).value
      inputRef.val = targetValue
      // 向父元素抛出 v-model 的 value 值
      context.emit('update:modelValue', targetValue)
    }
    const validateInput = () => {
      if (props.rules) {
        const isAllPass = props.rules.every(rule => {
          let passed = true
          inputRef.message = rule.message
          switch (rule.type) {
            case 'required':
              passed = inputRef.val !== ''
              break
            case 'email':
              passed = emailReg.test(inputRef.val)
              break

            default:
              break
          }
          return passed
        })
        inputRef.error = !isAllPass
      }
    }
    return { inputRef, validateInput, updateValue }
  }
})
</script>
<style lang="less" scoped></style>
```

引入

![image-20210930155946188](https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue328.7e4reizjeko0.webp)

```vue
<template>
  <div class="container">
    <global-header :user="currentUser"></global-header>
    <!-- <ColumnList :list="testData" /> -->
    <form action="">
      <div class="mb-3">
        <label class="form-label">邮箱地址</label>
        <validate-input :rules="emailRules" v-model="emailVal"></validate-input>
        {{emailVal}}
      </div>
      <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">邮箱地址</label>
        <input
          type="email"
          class="form-control"
          id="exampleInputEmail1"
          v-model="emailRef.val"
          @blur="validateEmail"
        />
        <div class="form-text" v-if="emailRef.error">
          {{ emailRef.message }}
        </div>
      </div>
      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">密码</label>
        <input type="password" class="form-control" id="exampleInputPassword" />
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import 'bootstrap/dist/css/bootstrap.min.css'
import { defineComponent, reactive, ref } from 'vue'
import ColumnList, { ColomnProps } from './components/ColumnList.vue'
import ValidateInput, { RulesProp } from './components/ValidateInput.vue'
import GlobalHeader, { UserProps } from './components/GlobalHeader.vue'
const currentUser: UserProps = {
  isLogin: true,
  name: 'viking'
}
// import HelloWorld from './components/HelloWorld.vue'
const testData: ColomnProps[] = [
  {
    id: 1,
    title: 'test1的专栏',
    description:
      '这是的test1专栏，有一段非常有意思的简介，可以更新一下欧, 这是的test1专栏，有一段非常有意思的简介，可以更新一下欧',
    avatar:
      'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_150,w_150'
  },
  {
    id: 2,
    title: 'test2的专栏',
    description: '这是的test2专栏，有一段非常有意思的简介，可以更新一下欧',
    avatar:
      'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_100,w_100'
  },
  {
    id: 3,
    title: 'test3的专栏',
    description:
      '这是的test1专栏，有一段非常有意思的简介，可以更新一下欧 这是的test1专栏，有一段非常有意思的简介，可以更新一下欧'
    // avatar:
    //   'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_100,w_100'
  },
  {
    id: 4,
    title: 'test4的专栏',
    description: '这是的test2专栏，有一段非常有意思的简介，可以更新一下欧',
    avatar:
      'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_100,w_100'
  }
]
const emailReg = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
export default defineComponent({
  name: 'App',
  components: {
    // ColumnList,
    GlobalHeader,
    ValidateInput
    // HelloWorld
  },
  setup() {
    const emailVal = ref('viking')
    const emailRules: RulesProp = [
      { type: 'required', message: '电子邮箱地址不能为空' },
      { type: 'email', message: '请输入正确的电子邮箱格式' }
    ]
    const emailRef = reactive({
      val: '',
      error: false,
      message: ''
    })
    const validateEmail = () => {
      if (emailRef.val.trim() === '') {
        emailRef.error = true
        emailRef.message = 'can not be empty'
      } else if (!emailReg.test(emailRef.val)) {
        emailRef.error = true
        emailRef.message = 'should be valid email'
      }
    }

    return {
      testData,
      currentUser,
      emailRef,
      validateEmail,
      emailRules,
      emailVal
    }
  }
})
</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

#### 9. input 使用 $attrs 支持默认属性

直接绑定 placeholder ，是绑定到 template 的根元素 div 上：

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue0315.4mwhjbv5ed80.webp" alt="image-20210930172824062" style="zoom:100%;" />

![image-20210930172906905](https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue0316.2i8xnp9w87k0.webp)

所有用到 Attribute 方法和 $attrs：

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue317.6dr3akjbhbo0.webp" alt="image-20210930171958806" style="zoom:67%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue318.3v1ttbo65e00.webp" alt="image-20210930172049291" style="zoom:67%;" />

具体实现， 在 ValidateInput.vue 文件中

```vue
<template>
  <div class="validate-input-container pb-3">
    <input
      class="form-control"
      :class="{ 'is-invalid': inputRef.error }"
      :value="inputRef.val"
      @blur="validateInput"
      @input="updateValue"
      v-bind="$attrs"
    />
    <!-- 3. v-bind="$attrs" 直接绑定 -->
    <span v-if="inputRef.error" class="invalid-feedback">
      {{ inputRef.message }}
    </span>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, PropType } from 'vue'
const emailReg = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
interface RuleProp {
  type: 'required' | 'email';
  message: string;
}
export type RulesProp = RuleProp[];
export default defineComponent({
  props: {
    rules: Array as PropType<RulesProp>,
    modelValue: String
  },
  // 1. 禁止根元素继承
  inheritAttrs: false,
  setup(props, context) {
    // 2. attrs 在 context 中
    console.log(context.attrs)
    const inputRef = reactive({
      val: props.modelValue || '',
      error: false,
      message: ''
    })
    const updateValue = (e: KeyboardEvent) => {
      const targetValue = (e.target as HTMLInputElement).value
      inputRef.val = targetValue
      context.emit('update:modelValue', targetValue)
    }
    const validateInput = () => {
      if (props.rules) {
        const isAllPass = props.rules.every(rule => {
          let passed = true
          inputRef.message = rule.message
          switch (rule.type) {
            case 'required':
              passed = inputRef.val !== ''
              break
            case 'email':
              passed = emailReg.test(inputRef.val)
              break

            default:
              break
          }
          return passed
        })
        inputRef.error = !isAllPass
      }
    }
    return { inputRef, validateInput, updateValue }
  }
})
</script>
<style lang="less" scoped></style>
```

引用

```vue
<div class="mb-3">
    <label class="form-label">邮箱地址</label>
    <validate-input :rules="emailRules" v-model="emailVal"  placeholder="请输入邮箱地址" type="text"></validate-input>
</div>
```

#### 10. input 传值和提交验证

使用\<slot>插槽，引入mitt，使用on、off、emit传值

ValidateInput.vue文件

```vue
<template>
  <div class="validate-input-container pb-3">
    <input
      class="form-control"
      :class="{ 'is-invalid': inputRef.error }"
      :value="inputRef.val"
      @blur="validateInput"
      @input="updateValue"
      v-bind="$attrs"
    />
    <!-- v-bind="$attrs" 直接绑定 -->
    <span v-if="inputRef.error" class="invalid-feedback">
      {{ inputRef.message }}
    </span>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, PropType, onMounted } from 'vue'
import { emitter } from './ValidateForm.vue'

const emailReg = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
interface RuleProp {
  type: 'required' | 'email' | 'range';
  message: string;
  // min?:'';
  // max?:''
}
export type RulesProp = RuleProp[];
export default defineComponent({
  props: {
    rules: Array as PropType<RulesProp>,
    modelValue: String
  },
  // 禁止根元素继承
  inheritAttrs: false,
  setup(props, context) {
    // attrs 在 context 中
    const inputRef = reactive({
      val: props.modelValue || '',
      error: false,
      message: ''
    })
    const updateValue = (e: KeyboardEvent) => {
      const targetValue = (e.target as HTMLInputElement).value
      inputRef.val = targetValue
      context.emit('update:modelValue', targetValue)
    }
    const validateInput = () => {
      if (props.rules) {
        const isAllPass = props.rules.every(rule => {
          let passed = true
          inputRef.message = rule.message
          switch (rule.type) {
            case 'required':
              passed = inputRef.val !== ''
              break
            case 'email':
              passed = emailReg.test(inputRef.val)
              break

            default:
              break
          }
          return passed
        })
        inputRef.error = !isAllPass
        return isAllPass
      }
    }
    onMounted(() => {
      emitter.emit('form-item-created', validateInput)
    })
    return { inputRef, validateInput, updateValue }
  }
})
</script>
<style lang="less" scoped></style>
```

ValidateForm.vue 文件

```vue
<template>
  <div>
    <form class="validate-form-container">
      <slot name="default"></slot>
      <div class="submit-area" @click.prevent="submitForm">
        <slot name="submit">
          <button type="submit" class="btn btn-primary">提交</button>
        </slot>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, onUnmounted, PropType } from 'vue'
import mitt from 'mitt'
type Events = {
  'form-item-created': any;
};
type ValidateFunc = () => boolean;
export const emitter = mitt<Events>()
export default defineComponent({
  emits: ['form-submit'],
  setup(props, context) {
    const funcArr: ValidateFunc[] = []
    const submitForm = () => {
      const result = funcArr.map(func => func()).every(result => result)
      context.emit('form-submit', result)
    }
    const callback = (func: ValidateFunc) => {
      funcArr.push(func)
    }
    emitter.on('form-item-created', callback)
    onUnmounted(() => {
      emitter.off('form-item-created', callback)
    })
    return {
      submitForm
    }
  }
})
</script>
<style lang="less" scoped></style>
```

App.vue

```vue
<template>
  <div class="container">
    <global-header :user="currentUser"></global-header>
    <!-- <ColumnList :list="testData" /> -->
    <validate-form @form-submit="onFormSubmit">
      <div class="mb-3">
        <label class="form-label">邮箱地址</label>
        <validate-input
          :rules="emailRules"
          v-model="emailVal"
          placeholder="请输入邮箱地址"
          type="text"
          ref="inputRef"
        ></validate-input>
      </div>
      <div class="mb-3">
        <label class="form-label">密码</label>
        <validate-input
          :rules="passwordRules"
          v-model="passwordVal"
          placeholder="请输入密码"
          type="password"
        ></validate-input>
      </div>
      <template #submit>
        <span class="btn btn-danger">Submit</span>
      </template>
    </validate-form>
  </div>
</template>

<script lang="ts">
import 'bootstrap/dist/css/bootstrap.min.css'
import { defineComponent, reactive, ref } from 'vue'
import ColumnList, { ColomnProps } from './components/ColumnList.vue'
import ValidateInput, { RulesProp } from './components/ValidateInput.vue'
import ValidateForm from './components/ValidateForm.vue'
import GlobalHeader, { UserProps } from './components/GlobalHeader.vue'
const currentUser: UserProps = {
  isLogin: true,
  name: 'viking'
}
// import HelloWorld from './components/HelloWorld.vue'
const testData: ColomnProps[] = [
  {
    id: 1,
    title: 'test1的专栏',
    description:
      '这是的test1专栏，有一段非常有意思的简介，可以更新一下欧, 这是的test1专栏，有一段非常有意思的简介，可以更新一下欧',
    avatar:
      'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_150,w_150'
  },
  {
    id: 2,
    title: 'test2的专栏',
    description: '这是的test2专栏，有一段非常有意思的简介，可以更新一下欧',
    avatar:
      'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_100,w_100'
  },
  {
    id: 3,
    title: 'test3的专栏',
    description:
      '这是的test1专栏，有一段非常有意思的简介，可以更新一下欧 这是的test1专栏，有一段非常有意思的简介，可以更新一下欧'
    // avatar:
    //   'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_100,w_100'
  },
  {
    id: 4,
    title: 'test4的专栏',
    description: '这是的test2专栏，有一段非常有意思的简介，可以更新一下欧',
    avatar:
      'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5ee22dd58b3c4520912b9470.jpg?x-oss-process=image/resize,m_pad,h_100,w_100'
  }
]
// const emailReg = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
export default defineComponent({
  name: 'App',
  components: {
    // ColumnList,
    GlobalHeader,
    ValidateInput,
    ValidateForm
    // HelloWorld
  },
  setup() {
    const inputRef = ref<any>()
    const emailVal = ref('')
    const emailRules: RulesProp = [
      { type: 'required', message: '电子邮箱地址不能为空' },
      { type: 'email', message: '请输入正确的电子邮箱格式' }
    ]
    const passwordVal = ref('')
    const passwordRules: RulesProp = [
      { type: 'required', message: '密码不能为空' }
    ]
    const onFormSubmit = (result: boolean) => {
      console.log('result', inputRef.value.validateInput())
    }

    return {
      testData,
      currentUser,
      passwordVal,
      passwordRules,
      emailRules,
      emailVal,
      inputRef,
      onFormSubmit
    }
  }
})
</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

### 11.5 vue-router配置路由

安装路由，4.0版本以上

```bash
npm install vue-router@next --save
```

main.ts引入

```typescript
import { createApp } from 'vue'
import { createRouter, createWebHistory } from 'vue-router'
import Home from './views/Home.vue'
import Login from './views/Login.vue'
import ColumnDetail from './views/ColumnDetail.vue'
import App from './App.vue'

const routerHistory = createWebHistory()
const router = createRouter({
  history: routerHistory,
  routes: [
    {
      path: '/',
      name: 'home',
      component: Home
    },
    {
      path: '/login',
      name: 'login',
      component: Login
    },
    {
      path: '/column/:id',
      name: 'column',
      component: ColumnDetail
    }
  ]
})
const app = createApp(App)
app.use(router)
app.mount('#app')
```

\<router-link> 方式使用：

```	html
<router-link :to="`/column/${column.id}`"  class="btn btn-outline-primary">进入专栏</router-link>
```

TS 中使用：

```typescript
import { useRouter } from 'vue-router'
const router = useRouter()
router.push({ name: 'column', params: { id: 1 } })   
```

### 11.6 vuex 整合当前应用

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue319.4mje3aat5pu0.webp" alt="image-20211102175810134" style="zoom:67%;" />

#### 1. 安装

使用介绍

在main.ts中使用

```typescript
import { createApp } from 'vue'
import App from './App.vue'
import { createStore } from 'vuex'
const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    add (state) {
      state.count++
    }
  }
})
console.log('store', store.state.count)
store.commit('add')
console.log('store2', store.state.count)

const app = createApp(App)
app.mount('#app')

```

#### 2. state、mutation、getters的基本使用：

抽取store/index.ts

```typescript
import { createStore } from 'vuex'
import axios from 'axios'
import { testData, testPosts } from '@/datas/testData'

interface UserProps {
  isLogin: boolean;
  name?: string;
  id?: string;
  columnId?: number;
}
interface ImageProps {
  _id?: string;
  url?: string;
  createdAt?: string;
}
export interface ColumnProps {
  id: number;
  title: string;
  avatar?: string;
  description: string;
}
export interface PostProps {
  id: number;
  title: string;
  content: string;
  image?: string;
  createdAt: string;
  columnId: number;
}
export interface GlobalDataProps {
  columns: ColumnProps[];
  posts: PostProps[];
  user: UserProps;
}
const store = createStore<GlobalDataProps>({
  state: {
    columns: testData,
    posts: testPosts,
    user: {
      isLogin: false
    }
  },
  mutations: {
    login (state) {
      state.user = { ...state.user, isLogin: true, name: 'viking' }
    },
    createPost (state, newPost) {
      state.posts.push(newPost)
    },
    fetchColumns (state, rawData) {
      state.columns = rawData.data.list
    }
  },
  actions: {
    fetchColumns (context) {
      axios.get('/column').then(res => {
        context.commit('fetchColumns', res.data)
      })
    }
  },
  getters: {
    biggerColumnsLen (state) {
      return state.columns.filter(c => c.id > 2).length
    },
    getColumnById: state => (id: number) => {
      return state.columns.find(c => c.id === id)
    },
    getPostsByCid: state => (cid: number) => {
      return state.posts.filter(post => post.columnId === cid)
    }
  }
})
export default store
```

#### 3. store.ts 抽取及引用

在main.ts中引用

```typescript
import { createApp } from 'vue'
import axios from 'axios'
import App from './App.vue'
import '@/assets/styles/reset.css'
import router from '@/router/index'
import store from '@/store/index'

// axios.defaults.baseURL = 'http://localhost:7001/api/'

const app = createApp(App)
app.use(router)
app.use(store)
app.mount('#app')
```

在组件中使用

```typescript
<script lang="ts">
import { computed, defineComponent } from 'vue'
import { testData } from '@/datas/testData'
import { useStore } from 'vuex'
import { GlobalDataProps } from '@/store/index'
import ColumnList from '../components/ColumnList.vue'
export default defineComponent({
  name: 'Home',
  components: {
    ColumnList
  },
  setup() {
    const store = useStore<GlobalDataProps>()
    const list = computed(() => store.state.columns)
    const biggerColumnLen = computed(() => store.getters.biggerColumnsLen)
    console.log(biggerColumnLen)

    return {
      list,
      biggerColumnLen
    }
  }
})
</script>
```

#### 4. actions 的基本使用和嵌套调用

登录获取token和获取用户信息

store/index.ts文件

```typescript
import { createStore, Commit } from 'vuex'
import axios from 'axios'
import { testData, testPosts } from '@/datas/testData'

export interface UserProps {
  isLogin: boolean;
  nickName?: string;
  _id?: string;
  column?: string;
  email?: string;
}
interface ImageProps {
  _id?: string;
  url?: string;
  createdAt?: string;
}
export interface ColumnProps {
  _id: string;
  title: string;
  avatar?: ImageProps;
  description: string;
}
export interface PostProps {
  _id: string;
  title: string;
  excerpt?: string;
  content?: string;
  image?: ImageProps;
  createdAt: string;
  column: string;
}
export interface GlobalDataProps {
  token: string;
  loading: boolean;
  columns: ColumnProps[];
  posts: PostProps[];
  user: UserProps;
}
const getAndCommit = async (
  url: string,
  mutationName: string,
  commit: Commit
) => {
  const { data } = await axios.get(`${url}?icode=73AC65D7AFBDB83B`)
  commit(mutationName, data)
}
const postAndCommit = async (
  url: string,
  mutationName: string,
  commit: Commit,
  payload: any
) => {
  const { data } = await axios.post(url, payload)
  commit(mutationName, data)
  return data
}
const store = createStore<GlobalDataProps>({
  state: {
    token: '',
    loading: false,
    columns: [],
    posts: [],
    user: {
      isLogin: false
    }
  },

  mutations: {
    // login (state) {
    //   state.user = { ...state.user, isLogin: true, name: 'viking' }
    // },
    createPost (state, newPost) {
      state.posts.push(newPost)
    },
    fetchColumns (state, rawData) {
      state.columns = rawData.data.list
    },
    fetchColumn (state, rawData) {
      state.columns = [rawData.data]
    },
    fetchPosts (state, rawData) {
      state.posts = rawData.data.list
    },
    setLoading (state, status) {
      state.loading = status
    },
    fetchCurrentUser (state, rawData) {
      state.user = {
        ...rawData.data,
        isLogin: true
      }
    },
    login (state, rawData) {
      const { token } = rawData.data
      state.token = token
      axios.defaults.headers.common.Authorization = `Bearer ${token}`
    }
  },
  actions: {
    fetchColumns ({ commit }) {
      getAndCommit('/columns', 'fetchColumns', commit)
    },
    fetchColumn ({ commit }, cid) {
      getAndCommit(`/columns/${cid}`, 'fetchColumn', commit)
    },
    fetchPosts ({ commit }, cid) {
      getAndCommit(`/columns/${cid}/posts`, 'fetchPosts', commit)
    },
    fetchCurrentUser ({ commit }) {
      getAndCommit('/user/current', 'fetchCurrentUser', commit)
    },
    login ({ commit }, payload) {
      return postAndCommit('/user/login', 'login', commit, payload)
    },
    // 登录和获取用户信息
    loginAntFetch ({ dispatch }, loginData) {
      return dispatch('login', loginData).then(() => {
        return dispatch('fetchCurrentUser')
      })
    }
  },
  getters: {
    biggerColumnsLen (state) {
      return state.columns.filter(c => +c._id > 2).length
    },
    getColumnById: state => (id: string) => {
      return state.columns.find(c => c._id === id)
    },
    getPostsByCid: state => (cid: string) => {
      return state.posts.filter(post => post.column === cid)
    }
  }
})
export default store
```

登录使用：

```vue
<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import { useStore } from 'vuex'
export default defineComponent({
  name: 'Login',
  setup() {
    	const emailVal = ref('')
    	const router = useRouter()
    	const store = useStore()
    	const onFormSubmit = (result: boolean) => {
      const payload = {
        email: emailVal.value,
        password: passwordVal.value
      }
      store.dispatch('loginAntFetch', payload).then(data => {
        router.push('/')
      })
    }
    return {
      inputRef,
      emailVal,
      emailRules,
      passwordRules,
      passwordVal,
      onFormSubmit
    }
  }
})
</script>
```



### 11.7 axios的基本用法

http请求：

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue320.q0swyi549fk.webp" alt="image-20211104104749769" style="zoom:50%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue321.2tvlqnm2ioc0.webp" alt="image-20211104104820111" style="zoom:50%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue322.3d2bu8pz6xg0.webp" alt="image-20211104105329456" style="zoom:50%;" />

安装：

```bash
npm install axios --save
```

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue323.6iwslsxcye80.webp" alt="image-20211109172622243" style="zoom:67%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue324.24m6upcz4yww.webp" alt="image-20211109173708683" style="zoom:67%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue325.nqvpz9p8lo0.webp" alt="image-20211109173834629" style="zoom:67%;" />

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue323.6iwslsxcye80.webp" alt="image-20211109172809757" style="zoom:67%;" />

### 11.8 vue3.0 Teleport传送门

使用封装loader.vue

```typescript
// 动态生成元素  
	const node = document.createElement('div') 
  node.id = 'back'
  document.body.appendChild(node)
// 注销
  onUnmounted(() => {
    document.body.removeChild(node)
  })
```

```html
<teleport to="#back"></teleport> <!-- 传送门使用 --->
```

组件代码：

```vue
<template>
  <teleport to="#back">
    <div
      class="d-flex justify-content-center align-items-center h-100 w-100 loading-container"
      :style="{ backgroundColor: background || '' }"
    >
      <div class="loading-content">
        <div class="spinner-border text-primary" role="status">
          <span class="sr-only">{{ text || 'loading' }}</span>
        </div>
        <p v-if="text" class="text-primary small">{{ text }}</p>
      </div>
    </div>
  </teleport>
</template>

<script lang="ts">
import { defineComponent, onUnmounted } from 'vue'
export default defineComponent({
  props: {
    text: {
      type: String
    },
    background: {
      type: String
    }
  },
  setup() {
    const node = document.createElement('div')
    node.id = 'back'
    document.body.appendChild(node)
    onUnmounted(() => {
      document.body.removeChild(node)
    })
    return {}
  }
})
</script>
<style scoped>
.loading-container {
  background: rgba(255, 255, 255, 0.5);
  z-index: 100;
  position: fixed;
  top: 0;
  left: 0;
}
.loading-container {
  text-align: center;
}
</style>
```

引用：

```vue
<loader v-if="isLoading"></loader>

<script lang="ts">
	import Loader from './components/Loader.vue'
	export default defineComponent({
 	 name: 'App',
  	components: {
   	 Loader
  	},
	})
</script>
```





JWT的运行机制

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/vue301.2jofje3sbps0.webp" alt="image-20211119110651417" style="zoom:67%;" />

### 11.9 message组件改进成函数形式

Message.vue 组件

```vue
<template>
  <teleport to="#message">
    <div
      class="alert message-info fixed-top w-50 mx-auto d-flex justify-content-between mt-2"
      :class="classObject"
      v-if="isVisible"
    >
      <!-- {{classObject}} -->
      <span>{{ message }}</span>
      <button
        type="button"
        class="close"
        aria-label="Close"
        @click.prevent="hide"
      >
        <span aria-hidden="true">&times;</span>
      </button>
    </div>
  </teleport>
</template>

<script lang="ts">
import { defineComponent, PropType, ref } from 'vue'
import useDOMCreate from '../../hooks/useDOMCreate'
export type MessageType = 'success' | 'warning' | 'error' | 'default';
export default defineComponent({
  props: {
    message: String,
    type: {
      type: String as PropType<MessageType>,
      default: 'default'
    }
  },
  emits: ['close-message'],
  setup(props, context) {
    useDOMCreate('message')
    const isVisible = ref(true)
    const classObject = {
      'alert-success': props.type === 'success',
      'alert-warning': props.type === 'warning',
      'alert-danger': props.type === 'error',
      'alert-primary': props.type === 'default'
    }
    const hide = () => {
      isVisible.value = false
      context.emit('close-message', true)
    }
    return {
      classObject,
      isVisible,
      hide
    }
  }
})
</script>
<style lang="less" scoped></style>
```

createMessage.ts 函数

```typescript
import { createApp } from 'vue'
import Message from './Message.vue'
// import type MessageType from '../Message.vue'
export type MessageType = 'success' | 'warning' | 'error' | 'default';
const createMessage = (message: string, type: MessageType, timeout = 2000) => {
  const messageInstance = createApp(Message, {
    message,
    type
  })
  const mountNode = document.createElement('div')
  document.body.appendChild(mountNode)
  messageInstance.mount(mountNode)
  setTimeout(() => {
    messageInstance.unmount()
    document.body.removeChild(mountNode)
  }, timeout)
}
export default createMessage
```

引入 App.vue

```vue
<template>
  <div class="container">
    <global-header :user="currentUser"></global-header>
    <loader v-if="isLoading"></loader>
    <router-view></router-view>
    <footer class="text-center py-4 text-secondary bg-light mt-6">
      <small>
        <ul class="list-inline mb-0">
          <li class="list-inline-item">© 2020 者也专栏</li>
          <li class="list-inline-item">课程</li>
          <li class="list-inline-item">文档</li>
          <li class="list-inline-item">联系</li>
          <li class="list-inline-item">更多</li>
        </ul>
      </small>
    </footer>
  </div>
</template>

<script lang="ts">
import 'bootstrap/dist/css/bootstrap.min.css'
import {
  computed,
  defineComponent,
  reactive,
  ref,
  onMounted,
  watch
} from 'vue'
import { useStore } from 'vuex'
import { GlobalDataProps } from '@/store/index'
import GlobalHeader from './components/GlobalHeader.vue'
import Loader from './components/Loader.vue'
// import Message from './components/Message.vue'
import createMessage from './components/message/createMessage'
import axios from 'axios'
export default defineComponent({
  name: 'App',
  components: {
    GlobalHeader,
    Loader
    // Message
  },
  setup() {
    const store = useStore<GlobalDataProps>()
    const currentUser = computed(() => store.state.user)
    const isLoading = computed(() => store.state.loading)
    const token = computed(() => store.state.token)
    const error = computed(() => store.state.error)
    onMounted(() => {
      if (!currentUser.value.isLogin && token.value) {
        axios.defaults.headers.common.Authorization = `Bearer ${token.value}`
        store.dispatch('fetchCurrentUser')
      }
    })
    watch(
      () => error.value.status,
      () => {
        const { status, message } = error.value
        if (status && message) {
          createMessage(message, 'error')
        }
      }
    )
    return {
      currentUser,
      isLoading,
      error
    }
  }
})
</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

引入 Login.vue

```typescript
import createMessage from '@/components/message/createMessage'
store.dispatch('loginAntFetch', payload).then(data => {
	createMessage('登录成功 2秒后跳转首页', 'success')
		setTimeout(() => {
			router.push('/')
		}, 2000)
	}).catch(e => {
	console.log(e)
})
```

