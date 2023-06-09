# hhjvuemusic重构

### hhjvuemusic@0.1.7 发现在vue3中使用不了，因为是2年前编写的组件库，使用webpack进行打包构建，可能一些构建上的语法不兼容vue3，这次重构就用了rollup进行打包构建，进测试，兼容vue2和vue3，且包体积减少了。


### hhjvuemusic@0.1.7

**下载**

```bash
npm i hhjvuemusic@0.1.7 -D
```

使用：

```js
<template>
  <div id="app">
    <hj-vue-music
     :musicData="musicData"
    ></hj-vue-music>
  </div>
</template>
<script>
  import HjVuemusic from 'hhjvuemusic'
import 'hhjvuemusic/dist/hhjvuemusic.css'
export default {
   components:{HjVuemusic},
  data() {
    return {
      musicData: [
        {
          id:1,
          musicName: "你就不要想起我",
          musicCover:
            "https://p1.music.126.net/m9psWT0vrunTgxY3NeEXUg==/109951164694992804.jpg",
          musicUrl: require("./assets/a.mp3"),
        },
        {
          id:2,
          musicName: "寂寞寂寞就好",
          musicCover:
            "https://p1.music.126.net/F_TTMR927naM6J-GzPP3RA==/109951164363518103.jpg",
          musicUrl: require("./assets/b.mp3"),
        },
      ],
    };
  },
};
</script>
<style lang="scss">
</style>

```

## 重构版本
### hhjvuemusic@latest

**下载**

```bash
npm i hhjvuemusic@latest -D
```

**使用**
```js
//vue2
<template>
    <div>
          <hj-vue-music :musicData="musicData"></hj-vue-music>
    </div>
</template>
<script>
import HjVuemusic from 'hhjvuemusic/dist/vue2/esm'

export default{
    components:{HjVuemusic},
   data() {
    return {
      musicData: [
        {
          id:1,
          musicName: "你就不要想起我",
          musicCover:
            "https://p1.music.126.net/m9psWT0vrunTgxY3NeEXUg==/109951164694992804.jpg",
          musicUrl: require("./assets/a.mp3"),
        },
        {
          id:2,
          musicName: "寂寞寂寞就好",
          musicCover:
            "https://p1.music.126.net/F_TTMR927naM6J-GzPP3RA==/109951164363518103.jpg",
          musicUrl: require("./assets/b.mp3"),
        },
      ],
    };
  },
}
</script>
```
```js
//vue3
<template>
    <div>
           <hj-vue-music :musicData="musicData"></hj-vue-music>

    </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'

import HjVueMusic from 'hhjvuemusic'

const musicData = ref([
  {
    id: 2,
    musicName: "寂寞寂寞就好",
    musicCover:
      "https://p1.music.126.net/F_TTMR927naM6J-GzPP3RA==/109951164363518103.jpg",
    musicUrl: require("./assets/b.mp3"),
  },
])
</script>
```

参数说明：`musicData`

| 字段       | 是否必传 | 值类型          |
| ---------- | -------- | --------------- |
| id         | 是       | Number / String |
| musicName  | 是       | String          |
| musicCover | 是       | String          |
| musicUrl   | 是       | String          |

## 效果展示：

![](https://i.loli.net/2021/05/07/AUBNHwbIfZjVMXu.png)
