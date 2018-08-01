# 华夏实时通讯-客户端-项目开发规范

## 命名规范
### 1. 组件
###### 推荐方式
  >* 当注册组件 (或者 prop) 时，可以使用 kebab-case (短横线分隔命名)、camelCase (驼峰式命名) 或 PascalCase (单词首字母大写命名)。
  >* PascalCase 是最通用的声明约定而 kebab-case 是最通用的使用约定。
###### 注册组件
`PascalCase`
###### 使用组件
`kebab-case`
###### 命名可遵循以下规则
  >* 有意义的名词、简短、具有可读性
  >* 以小写开头，采用短横线分割命名
  >* 文件夹命名主要以功能模块代表命名
### 2. 路由
###### 命名可遵循以下规则
  >* 全部小写
  >* 以小写开头，采用短横线分割命名
  >* 尽量按照功能模块来划分
### 3. 方法
###### 组件方法  *(参与dom交互的方法，包括用户操作可能调用的方法，可在其他组件内部调用)*
`myFunction()`
###### 内置方法  *(用于处理内部数据，只能在当前组件内部使用的方法)*
`_myFunction()`

## 结构化规范
###### vue文件基本结构
```
<template>
  <div class="当前组件的名称">
    <!--必须在div中编写页面-->
  </div>
</template>
<script>
  export default {
    components: {

    },
    data() {
      return {

      }
    },
    mounted() {

    },
    methods: {

    }
 }
</script>
<!--声明语言，并且添加scoped-->
<style lang="less" scoped>

</style>
```
###### vue文件方法声明顺序
```
  - components   
  - props    
  - data     
  - created
  - mounted
  - activited
  - update
  - beforeRouteUpdate
  - metods   
  - filter
  - computed
  - watch
```

## 路由
###### 主要按照功能模块划分
  * chatRoom (聊天室模块)
    * sendGift (发礼物)
    * sendExpress (发表情)
    * extend (发文件)
  * videoRoom (视频聊天模块)
    * lineUp (排队)
    * csVideo (视频)
  * csDetail (客服详情)
  * share (分享)
  * error (网络状态差 & 系统版本低)

## 组件
### 1. 组件加载规范
###### 尽可能使用异步加载
  >* 加载组件时，尽可能使用异步`import()`方法
  ```
    const main = () => import('@/views/mainChat')

    export default new Router({
      mode: 'history',
      routes: [
        {
          path: '/',
          name: 'main',
          component: main
        }
      ],
      base: '/web/'
    })
  ```
  ```
    components: {
      'HeaderBar': () => import('@/views/components/header-bar'),
      'ChatContentItem': () => import('@/views/components/chat-content-item'),
      'FloadButton': () => import('@/views/components/fload-button')
    },
  ```
  >* 注册组件时，也可考虑使用 webpack 4 新增的`require.ensure`方法
  ```
    const mainM = require('@/views/mainChat')
    const main = r => require.ensure([], () => r(mainM), 'main')
  ```
### 2. 组件划分
###### 公用组件
  ```
    - header-bar.vue // 头部组件
    - gift-Modal.vue // 赠送礼物时的弹出层
    - assess-Modal.vue // 客服评价弹出层
  ```
###### chatRoom模块内部组件
  ```
  // 主路由
    - chat-content-item.vue // 对话消息组件
    - chat-bot-output.vue // 机器人返回输入组件
    - chat-tip-item.vue // 对话提示组件(时间 & 转接 & 状态)
    - fload-button.vue // 右侧浮动按钮
    - input-bar.vue // 底部输入区域

  // sendGift路由
    - send-gift.vue // 底部发送礼物

  // sendExpress路由
    - send-express.vue // 底部发送表情

  // extend路由
    - send-extend.vue // 底部发送文件
  ```
