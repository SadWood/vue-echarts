# Vue-ECharts

> Apache ECharts 的 Vue.js 组件。

使用 [Apache ECharts](https://echarts.apache.org/zh/index.html) 5，同时支持 [Vue.js](https://vuejs.org/) 2/3。

## 💡 注意 💡

若您准备从 `vue-echarts` ≤ 5 的版本迁移到新版本，请在升级 v6 前阅读 _[迁移到 v6](#迁移到-v6)_ 部分文档。

没准备好的话，可以继续阅读老版本的文档。[前往 →](https://github.com/ecomfe/vue-echarts/blob/5.x/README.zh_CN.md)

## 安装 & 使用

### npm & ESM

```bash
$ npm install echarts vue-echarts
```

要在 *Vue 2*（<2.7.0）下使用 `vue-echarts`，需要确保 `@vue/composition-api` 已经安装：

```sh
npm i -D @vue/composition-api
```

如果你在使用基于 *Vue 2*（<2.7.0）的 *NuxtJS*，那么还需要安装 `@nuxtjs/composition-api`：

```sh
npm i -D @nuxtjs/composition-api
```

然后在 `nuxt.config.js` 的 `buildModules` 选项中添加 `'@nuxtjs/composition-api/module'`。

<details>
<summary>Vue 3</summary>

```js
import { createApp } from 'vue'
import ECharts from 'vue-echarts'
import { use } from "echarts/core";

// 手动引入 ECharts 各模块来减小打包体积
import {
  CanvasRenderer
} from 'echarts/renderers'
import {
  BarChart
} from 'echarts/charts'
import {
  GridComponent,
  TooltipComponent
} from 'echarts/components'

use([
  CanvasRenderer,
  BarChart,
  GridComponent,
  TooltipComponent
]);

const app = createApp(...)

// 全局注册组件（也可以使用局部注册）
app.component('v-chart', ECharts)

app.mount(...)
```

</details>

<details>
<summary>Vue 2</summary>

```js
import Vue from 'vue'
import ECharts from 'vue-echarts'
import { use } from 'echarts/core'
// 手动引入 ECharts 各模块来减小打包体积

import {
  CanvasRenderer
} from 'echarts/renderers'
import {
  BarChart
} from 'echarts/charts'
import {
  GridComponent,
  TooltipComponent
} from 'echarts/components'

use([
  CanvasRenderer,
  BarChart,
  GridComponent,
  TooltipComponent
]);

// 全局注册组件（也可以使用局部注册）
Vue.component('v-chart', ECharts)

new Vue(...)
```

</details>

为了更小的打包体积，我们建议手动从 ECharts 引入单个图表和组件。请参考所有支持的渲染器/图表/组件。[前往 →](https://github.com/apache/echarts/blob/master/src/echarts.all.ts)

但如果你实在需要全量引入 ECharts 从而无需手动引入模块，只需要在代码中添加：

```js
import "echarts";
```

#### 单文件组件示例

<details>
<summary>Vue 3 <a href="https://codesandbox.io/s/charming-night-2y6m6?file=/src/App.vue">Demo →</a></summary>

```vue
<template>
  <v-chart class="chart" :option="option" />
</template>

<script>
import { use } from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";
import { PieChart } from "echarts/charts";
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent
} from "echarts/components";
import VChart, { THEME_KEY } from "vue-echarts";
import { ref, defineComponent } from "vue";

use([
  CanvasRenderer,
  PieChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent
]);

export default defineComponent({
  name: "HelloWorld",
  components: {
    VChart
  },
  provide: {
    [THEME_KEY]: "dark"
  },
  setup: () => {
    const option = ref({
      title: {
        text: "Traffic Sources",
        left: "center"
      },
      tooltip: {
        trigger: "item",
        formatter: "{a} <br/>{b} : {c} ({d}%)"
      },
      legend: {
        orient: "vertical",
        left: "left",
        data: ["Direct", "Email", "Ad Networks", "Video Ads", "Search Engines"]
      },
      series: [
        {
          name: "Traffic Sources",
          type: "pie",
          radius: "55%",
          center: ["50%", "60%"],
          data: [
            { value: 335, name: "Direct" },
            { value: 310, name: "Email" },
            { value: 234, name: "Ad Networks" },
            { value: 135, name: "Video Ads" },
            { value: 1548, name: "Search Engines" }
          ],
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: "rgba(0, 0, 0, 0.5)"
            }
          }
        }
      ]
    });

    return { option };
  }
});
</script>

<style scoped>
.chart {
  height: 400px;
}
</style>
```

</details>

<details>
<summary>Vue 2 <a href="https://codesandbox.io/s/suspicious-glitter-mk66j?file=/src/App.vue">Demo →</a></summary>

```vue
<template>
  <v-chart class="chart" :option="option" />
</template>

<script>
import { use } from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";
import { PieChart } from "echarts/charts";
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent
} from "echarts/components";
import VChart, { THEME_KEY } from "vue-echarts";

use([
  CanvasRenderer,
  PieChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent
]);

export default {
  name: "HelloWorld",
  components: {
    VChart
  },
  provide: {
    [THEME_KEY]: "dark"
  },
  data() {
    return {
      option: {
        title: {
          text: "Traffic Sources",
          left: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: "left",
          data: [
            "Direct",
            "Email",
            "Ad Networks",
            "Video Ads",
            "Search Engines"
          ]
        },
        series: [
          {
            name: "Traffic Sources",
            type: "pie",
            radius: "55%",
            center: ["50%", "60%"],
            data: [
              { value: 335, name: "Direct" },
              { value: 310, name: "Email" },
              { value: 234, name: "Ad Networks" },
              { value: 135, name: "Video Ads" },
              { value: 1548, name: "Search Engines" }
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            }
          }
        ]
      }
    };
  }
};
</script>

<style scoped>
.chart {
  height: 400px;
}
</style>
```

</details>

### CDN & 全局变量

用如下方式在 HTML 中插入 `<script>` 标签，并且通过 `window.VueECharts` 来访问组件接口：

<details>
<summary>Vue 3 <!-- vue3DemoLink:start --><a href="https://codesandbox.io/api/v1/sandboxes/define?parameters=N4IgZglgNgpgziAXKCA7AJjAHgOgBYAuAtlEqAMYD2qBMNSIAPAIQAiA8gMIAqAmgAoBRAASESAPgA6qRmKhSZeGAEN0CxnAIBPWAoDEygA6HhwacNEwIAc0KJhAFgAMTw1gDc0gL7TGAek0dGHU_JVV1ACNKdC11dAgAN2EIdABeSRAjQwyFC0YEgFpyPGUAJwJhZQBXAkpS-AgALxhhREpDAghqdJB2zuoMvxD4hPU4clKIDuE4UvIewgJDOEQ_P3J0VBwAKzhMKETSnFQYAj9UQyI_BKqYAAEAZhwAJhwHgHYc_3HJjrGJqYVWbzDKLZardabHZ7GAHBJHE5nC5XGDFMoEOB3ACsbzeXwCAL-vh-gJmcwWBCWKzWGy2u32h2Op3Ol2utwKqJK5UxADYXjgAIz4klEmQiggKABqtxwExUtAAgsYABRmVAWdDKAjKZUASlM5gswnqBCqpXVaqNRr6XVQ9ktVqNtCwBAAytpYPbDY6jWBqAQAGLKIjQLT2ADkAEkaDBSgAaYQZAASsISpwg5GUwgAcjBbhkEwrJsooAm4MpUHACnBYxAwOHvY6vHHG1bOgRPQb1T6ndgCPYMtxSsowJByMJXZQzeR4AXW47YGB-4mQDOY6UMvOLM2t8JapQoJ1DF7uz295NrNZYwOQBBaEQ56ee37SkQtbRSjfgMovMJGBFSiGYAIl_e1yF_VV0C8ABSXVNyfK0dwQo1YCvDATzPCw6ggOhlwyNNygzEtH0w4RFzwkByJIzDNW1ewAG1dytDJWAgepyAIajSIyQQ32gLjMIyBV0BzU4AHc6gAazgASzwySUUhgShhGEmSQBbZCfQyV0VDmPBhEEVBrDQWcQCY4QAF1dyQs8a0meAGPMh1SNQYMYBvIcRzHCcpzmUyNNIixtEMdyV0MHDZJ7Yd4iqFYVyxLFoMin01w_RyQCxJwkvUlceSyjILICwLaOURzNJ7YBhASEtbnsB4HixBNXKIUKWLY1FOJAYQbMCixKuqqBauEB4BScJq3JvXjlH4rqet6_qatC54HgccaWpvYTRIICTSmkjJuqK-aqsW-wBQatbWpABTMGU1T9rmwKFsG0KBSxBwAA4LpvHSymKAyjJMtTuvMixCpB4QYCIQwSjgCA4ucwK70h90ggw3qjTgEp0EoMSACFBs_YRRsO9HMdUHH2FHGsCAADXsMbwatMnsbEzgDzqG9SmsCIdTG4Q-YFnAsTgszyp9HwxaNCXSOlnsrOQ2Xt08bsJa8ODu1lSgoeoXDlXwoouU6hNpRgQROENuB1YsHAiCnGg9ZAAxjAyXVle-QkJV8PwohiEI5HEEAvCDoA">Demo →</a><!-- vue3DemoLink:end --></summary>

<!-- vue3Scripts:start -->
```html
<script src="https://cdn.jsdelivr.net/npm/vue@3.2.37"></script>
<script src="https://cdn.jsdelivr.net/npm/echarts@5.3.3"></script>
<script src="https://cdn.jsdelivr.net/npm/vue-echarts@6.2.1"></script>
```
<!-- vue3Scripts:end -->

```js
const app = Vue.createApp(...)

// 全局注册组件（也可以使用局部注册）
app.component('v-chart', VueECharts)
```

</details>

<details>
<summary>Vue 2 <!-- vue2DemoLink:start --><a href="https://codesandbox.io/api/v1/sandboxes/define?parameters=N4IgZglgNgpgziAXKCA7AJjAHgOgBYAuAtlEqAMYD2qBMNSIAPAIQAiA8gMIAqAmgAoBRAASESAPgA6qRmKhSZeGAEN0CxnAIBPWAoDEygA6HhwacNEwIAc0KJhAFgAMTw1gDc0gL7TGAek0dGHU_JVV1ACNKdC11dAgAN2EIdABeSRAjQwyFC0YEgFpyPGUAJwJhZQBXAkpS-AgALxhhREpDAghqdJB2zuoMvxD4hPU4clKIDuE4UvIewgJDOEQ_P3J0VBwAKzhMKETSnFQYAj9UQyI_BKqYAAEAJhwAdhwAZhz_ccmOsYmpiqzeYZRbLVbrTY7PYwA4JI4nM4XK4wYplAhwO4AVne70-AX-v183wBMzmCwISxWaw2W12-0Ox1O50u11uBRRJXKGIAbDgngBGPHEwkyYUEBQANVuOCoREM1DoBAAFBlCqjyhkADTCKUwQScTnogCUnlQ0hOAHcdbclWZUBYYfYMgZjFrzMJ0MoCMolUbTO6LPUCFVSva7RYI8I-l1UPZw5GI7QsAQAMraWBxgMJixgagEABiyiI0C09gA5ABJGgwUrajIACRhCVOEHIymEADkYLctcIAIKTZRQbVwZSoOAFOA1iBgMtZyNeTXzxMQAgZ_327OJ7AEJ0gbilZRgSDkYQpygh8jwN2brfCWBgXfCDJX6ulDLLiyLz_CWqUKCdIYma3luBCTNY1g1nuq4wEQN53jmdREF6tClHuwDKF4wiMBEpRDMAERYXG5BYba6BeAApEaH4gQuS60RGsCQRgwEIVGkyKnuzblK2Q7wQhD5Phkgn8XenrevYADaP6RhkrAQPU5AEKJCEZIIyHQCpd4ZH26CdqcFp1AA1nAWlbhkEopDAlD9ugpkgPRbEWBkKYqHMeDCIIqDWGg14gDJwgALo_t-DEWFOHErMI0lhRG8ZsagRYwHuB5HieZ4XnMfmOU52iGMlz4gIYEAwGZ2aHvEVRRRkmKYhRZUJq-qFSTVTj1Q5hXcm1GSBTlbHicoLWxdmwDCAkQ63PYbxvJi2qJUQBVyQpKLKSAwihU5cVjRNBVvPyThzUle7qcomlrRtm2mNtUCTcIDxvA4h0LXuun6QQhmlCZGTrX1TmjeNN0FfyM1PYtICWZgNm6fZP0BZG_07fY_KYg4AAcoN7q5ZTFJ53m-TDPjDRGvVw8IsGGCUcAQFF8VOTBRBpkErGXeFJToJQFoAEI3Whwj7b9m1wGzHPsMeU4EAAGvYB2k5GQuqBznD_nUe6lNYEQ-gdwhazrOCYtR_lEwupOE2xptbsFDHm1-ppft4Jq-PiPzio7UQxCEcjiCAXg-0AA">Demo →</a><!-- vue2DemoLink:end --></summary>

<!-- vue2Scripts:start -->
```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.3"></script>
<script src="https://cdn.jsdelivr.net/npm/echarts@5.3.3"></script>
<script src="https://cdn.jsdelivr.net/npm/vue-echarts@6.2.1"></script>
```
<!-- vue2Scripts:end -->

```js
// 全局注册组件（也可以使用局部注册）
Vue.component("v-chart", VueECharts);
```

</details>

可以在[这里](https://github.com/ecomfe/vue-echarts/tree/main/src/demo)查看更多例子。

### Prop

- `init-options: object`

  初始化附加参数。请参考 `echarts.init` 的 `opts` 参数。[前往 →](https://echarts.apache.org/zh/api.html#echarts.init)

  Inject 键名：`INIT_OPTIONS_KEY`。

- `theme: string | object`

  要应用的主题。请参考 `echarts.init` 的 `theme` 参数。[前往 →](https://echarts.apache.org/zh/api.html#echarts.init)

  Inject 键名：`THEME_KEY`。

- `option: object`

  ECharts 的万能接口。修改这个 prop 会触发 ECharts 实例的 `setOption` 方法。查看[详情 →](https://echarts.apache.org/zh/option.html)

  > 💡 在没有指定 `update-options` 时，如果直接修改 `option` 对象而引用保持不变，`setOption` 方法调用时将默认指定 `notMerge: false`；否则，如果为 `option` 绑定一个新的引用，将指定 `notMerge: true`。

- `update-options: object`

  图表更新的配置项。请参考 `echartsInstance.setOption` 的 `opts` 参数。[前往 →](https://echarts.apache.org/zh/api.html#echartsInstance.setOption)

  Inject 键名：`UPDATE_OPTIONS_KEY`。

- `group: string`

  图表的分组，用于[联动](https://echarts.apache.org/zh/api.html#echarts.connect)。请参考 `echartsInstance.group`。[前往 →](https://echarts.apache.org/zh/api.html#echartsInstance.group)

- `autoresize: boolean`（默认值`false`）

  图表在组件根元素尺寸变化时是否需要自动进行重绘。

- `loading: boolean`（默认值：`false`）

  图表是否处于加载状态。

- `loading-options: object`

  加载动画配置项。请参考 `echartsInstance.showLoading` 的 `opts` 参数。[前往 →](https://echarts.apache.org/zh/api.html#echartsInstance.showLoading)

  Inject 键名：`LOADING_OPTIONS_KEY`。

- `manual-update: boolean`（默认值`false`）

  在性能敏感（数据量很大）的场景下，我们最好对于 `option` prop 绕过 Vue 的响应式系统。当将 `manual-update` prop 指定为 `true` 且不传入 `option` prop 时，数据将不会被监听。然后，需要用 `ref` 获取组件实例以后手动调用 `setOption` 方法来更新图表。

### 事件

可以使用 Vue 的 `v-on` 指令绑定事件。

```vue
<template>
  <v-chart :option="option" @highlight="handleHighlight"/>
</template>
```

> 仅支持 `.once` 修饰符，因为其它修饰符都与 DOM 事件机制强耦合。

Vue-ECharts 支持如下事件：

- `highlight` [→](https://echarts.apache.org/zh/api.html#events.highlight)
- `downplay` [→](https://echarts.apache.org/zh/api.html#events.downplay)
- `selectchanged` [→](https://echarts.apache.org/zh/api.html#events.selectchanged)
- `legendselectchanged` [→](https://echarts.apache.org/zh/api.html#events.legendselectchanged)
- `legendselected` [→](https://echarts.apache.org/zh/api.html#events.legendselected)
- `legendunselected` [→](https://echarts.apache.org/zh/api.html#events.legendunselected)
- `legendselectall` [→](https://echarts.apache.org/zh/api.html#events.legendselectall)
- `legendinverseselect` [→](https://echarts.apache.org/zh/api.html#events.legendinverseselect)
- `legendscroll` [→](https://echarts.apache.org/zh/api.html#events.legendscroll)
- `datazoom` [→](https://echarts.apache.org/zh/api.html#events.datazoom)
- `datarangeselected` [→](https://echarts.apache.org/zh/api.html#events.datarangeselected)
- `timelinechanged` [→](https://echarts.apache.org/zh/api.html#events.timelinechanged)
- `timelineplaychanged` [→](https://echarts.apache.org/zh/api.html#events.timelineplaychanged)
- `restore` [→](https://echarts.apache.org/zh/api.html#events.restore)
- `dataviewchanged` [→](https://echarts.apache.org/zh/api.html#events.dataviewchanged)
- `magictypechanged` [→](https://echarts.apache.org/zh/api.html#events.magictypechanged)
- `geoselectchanged` [→](https://echarts.apache.org/zh/api.html#events.geoselectchanged)
- `geoselected` [→](https://echarts.apache.org/zh/api.html#events.geoselected)
- `geounselected` [→](https://echarts.apache.org/zh/api.html#events.geounselected)
- `axisareaselected` [→](https://echarts.apache.org/zh/api.html#events.axisareaselected)
- `brush` [→](https://echarts.apache.org/zh/api.html#events.brush)
- `brushEnd` [→](https://echarts.apache.org/zh/api.html#events.brushEnd)
- `brushselected` [→](https://echarts.apache.org/zh/api.html#events.brushselected)
- `globalcursortaken` [→](https://echarts.apache.org/zh/api.html#events.globalcursortaken)
- `rendered` [→](https://echarts.apache.org/zh/api.html#events.rendered)
- `finished` [→](https://echarts.apache.org/zh/api.html#events.finished)
- 鼠标事件
  - `click` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.click)
  - `dblclick` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.dblclick)
  - `mouseover` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.mouseover)
  - `mouseout` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.mouseout)
  - `mousemove` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.mousemove)
  - `mousedown` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.mousedown)
  - `mouseup` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.mouseup)
  - `globalout` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.globalout)
  - `contextmenu` [→](https://echarts.apache.org/zh/api.html#events.Mouse%20events.contextmenu)
- ZRender 事件
  - `zr:click`
  - `zr:mousedown`
  - `zr:mouseup`
  - `zr:mousewheel`
  - `zr:dblclick`
  - `zr:contextmenu`

请参考支持的事件列表。[前往 →](https://echarts.apache.org/zh/api.html#events)

### Provide / Inject

Vue-ECharts 为 `theme`、`init-options`、`update-options` 和 `loading-options` 提供了 provide/inject API，以通过上下文配置选项。例如：可以通过如下方式来使用 provide API 为 `init-options` 提供上下文配置：

<details>
<summary>Vue 3</summary>

```js
import { INIT_OPTIONS_KEY } from 'vue-echarts'
import { provide } from 'vue'

// composition API
provide(INIT_OPTIONS_KEY, ...)

// options API
{
  provide: {
    [INIT_OPTIONS_KEY]: { ... }
  }
}
```

</details>

<details>
<summary>Vue 2</summary>

```js
import { INIT_OPTIONS_KEY } from 'vue-echarts'

// in component options
{
  provide: {
    [INIT_OPTIONS_KEY]: { ... }
  }
}
```

</details>

### 方法

- `setOption` [→](https://echarts.apache.org/zh/api.html#echartsInstance.setOption)
- `getWidth` [→](https://echarts.apache.org/zh/api.html#echartsInstance.getWidth)
- `getHeight` [→](https://echarts.apache.org/zh/api.html#echartsInstance.getHeight)
- `getDom` [→](https://echarts.apache.org/zh/api.html#echartsInstance.getDom)
- `getOption` [→](https://echarts.apache.org/zh/api.html#echartsInstance.getOption)
- `resize` [→](https://echarts.apache.org/zh/api.html#echartsInstance.resize)
- `dispatchAction` [→](https://echarts.apache.org/zh/api.html#echartsInstance.dispatchAction)
- `convertToPixel` [→](https://echarts.apache.org/zh/api.html#echartsInstance.convertToPixel)
- `convertFromPixel` [→](https://echarts.apache.org/zh/api.html#echartsInstance.convertFromPixel)
- `containPixel` [→](https://echarts.apache.org/zh/api.html#echartsInstance.containPixel)
- `showLoading` [→](https://echarts.apache.org/zh/api.html#echartsInstance.showLoading)
- `hideLoading` [→](https://echarts.apache.org/zh/api.html#echartsInstance.hideLoading)
- `getDataURL` [→](https://echarts.apache.org/zh/api.html#echartsInstance.getDataURL)
- `getConnectedDataURL` [→](https://echarts.apache.org/zh/api.html#echartsInstance.getConnectedDataURL)
- `clear` [→](https://echarts.apache.org/zh/api.html#echartsInstance.clear)
- `dispose` [→](https://echarts.apache.org/zh/api.html#echartsInstance.dispose)

### 静态方法

静态方法请直接通过 [`echarts` 本身](https://echarts.apache.org/zh/api.html#echarts)进行调用。

## 迁移到 v6

> 💡 请确保同时查阅 ECharts 5 的[升级指南](https://echarts.apache.org/zh/tutorial.html#ECharts%205%20%E5%8D%87%E7%BA%A7%E6%8C%87%E5%8D%97)。

`vue-echarts@6` 引入了如下破坏性变更：

### Vue 2 支持

- 要在 `vue@2.7.0` 之前的版本中使用 Vue-ECharts，必须安装 `@vue/composition-api`。

### Prop

- `options` 重命名为 **`option`**，以和 ECharts 本身保持一致。
- 更新 `option` 将采用 **`update-options`** 中的配置，不再检查是否发生引用变化。
- `watch-shallow` 被移除。在性能关键场景请使用 **`manual-update`**。

### 方法

- `mergeOptions` 重命名为 **`setOption`**，以和 ECharts 本身保持一致。
- `showLoading` 与 `hideLoading` 被移除。请使用 **`loading` 与 `loading-options`** prop。
- `appendData` 被移除。（由于 ECharts 5 引入的破坏性变更。）
- 所有静态方法被从 `vue-echarts` 移除。可以直接使用 `echarts` 本身的这些方法。

### 计算 Getter

- 计算 getter（`width`、`height`、`isDisposed` 和 `computedOptions`）被移除。请分别使用 **`getWidth`、`getHeight`、`isDisposed` 和 `getOption`** 方法代替。

### 样式

- 现在组件根元素尺寸默认为 **`100%×100%`**，而非原来的 `600×400`。

## 本地开发

```bash
$ npm i
$ npm run serve
```

打开 `http://localhost:8080` 来查看 demo。
