# bluestar-PersonInfo

## 人员机构切换

## 引入

在script标签中引入组件

```typescript
//使用HBuilderX导入插件
import PersonInfo from "@/uni_modules/bluestar-PersonInfo/components/bluestar-PersonInfo/bluestar-PersonInfo.vue"

//使用npm install
import PersonInfo from "bluestar-personinfo/components/bluestar-PersonInfo/bluestar-PersonInfo.vue"
```

## 代码演示

```vue
<template>
	<PersonInfo isOrg @init="perChange"></PersonInfo>
</template>

<script setup lang="ts">
const perChange = (perInfo : any, orgInfo : any) => {
	console.log(perInfo)
	console.log(orgInfo)
}
</script>
```

## API

#### Props

| 参数   | 说明          | 类型           | 默认值/参考值                                |
|------|:------------|:-------------|:---------------------------------------|
| isOrg | 是否显示机构切换    | `boolean`    | false                                  |

#### Events

| 事件名称 | 说明              | 回调参数 |
| :----- |:----------------|------|
| bing:init | 人员或机构切换，点击确定时触发 | 选中值  |

## 作者

![](https://img.shields.io/static/v1?label=蓝星软件&message=@caisheng&labelColor=0E75FC)
