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
    * sendExtend (发文件)
  * videoRoom (视频聊天模块)
    * lineUp (排队)
    * csVideo (视频)
    * error (网络状态差 & 系统版本低)
  * csDetail (客服详情)
  * share (分享)

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
    - gift-Modal.vue // 赠送礼物时的弹出层
    - assess-Modal.vue // 客服评价弹出层
    - assess-label.vue // 客服评价标签
    - star-bar.vue // 评价星级组件
    - send-gift-item.vue // 礼物列表item
  ```
###### chatRoom模块内部组件
  ```
  // 入口路由组件
    - chatRoom.vue // 底部发送礼物

  // 公共组件
    - chat-header-bar.vue // 聊天头部组件
    - chat-content-item.vue // 对话消息组件
    - chat-bot-output.vue // 机器人返回输出组件
    - chat-tip-item.vue // 对话提示组件(时间 & 转接 & 状态)
    - fload-button.vue // 右侧浮动按钮
    - input-bar.vue // 底部输入区域

  // sendGift路由组件
    - sendGift.vue // 底部发送礼物

  // sendExpress路由组件
    - sendExpress.vue // 底部发送表情

  // extend路由组件
    - sendExtend.vue // 底部发送文件
  ```
###### videoRoom模块内部组件
  ```
  // 入口路由组件
    - videoRoom.vue // 底部发送礼物

  // 公共组件
    - confirm-to-video-Modal.vue // 确认进入视频客服弹出层
    - iOS-mask-Modal.vue // iOS转移至浏览器出层
    - success-Modal.vue // 转接成功弹出层
    - video-header-bar.vue // 视频头部组件
    - video-footer-bar.vue // 视频底部组件
    - finish-Modal.vue // 视频通话结束弹出层

  // lineUp路由组件
    - lineUp.vue // 排队

  // csVideo路由组件
    - csVideo.vue // 视频客服房间

  // error路由组件
    - error.vue // 底部发送文件
  ```
###### csDetail模块内部组件
  ```
  // 入口路由组件
    - csDetail.vue // 客服详情页
  ```
###### share模块内部组件
  ```
  // 入口路由组件
    - share.vue // 分享页

  // 公共组件
    - user-state-Modal.vue // 未关注官微 & 不是VIP
  ```

## 本地数据模型
###### chatRoom
首屏进入先获取历史消息记录列表(若没有则略过)，大概长这样：
  ```
  chat: [
    {
      groupId: '372331123',
      identifier: 'userid_web_1530869554913',
      nickName: '小华',
      msg: '尊贵的客人，您好！',
      textType: 0,
      time: '2018-03-28 08:45:19',
      type: 'text_msg'
    },
    {
      groupId: '372331123',
      identifier: 'userid_web_1530869554913',
      nickName: '客人',
      msg: 'hello！你好',
      textType: 1,
      time: '2018-03-28 08:45:19',
      type: 'text_msg'
    }
  ]
  ```
根据拿到的消息记录的时间字段 `time` ，插入时间msg，重新构建消息队列，并缓存最后一条消息的时间节点，之后的对话绘制根据新的消息队列 push 不同类型的消息，规则为以分钟为单位绘制消息tip，其他类型的tip根据具体场景绘制
  ```
  chat: [
    {
      time: '2018-03-28 08:45:19',
      type: 'time_msg'
    },
    {
      groupId: '372331123',
      identifier: 'userid_web_1530869554913',
      nickName: '小华',
      msg: '尊贵的客人，您好！',
      textType: 0,
      time: '2018-03-28 08:45:19',
      type: 'text_msg'
    },
    {
      groupId: '372331123',
      identifier: 'userid_web_1530869554913',
      nickName: '客人',
      msg: 'hello！你好',
      textType: 1,
      time: '2018-03-28 08:45:19',
      type: 'text_msg'
    }
  ]
  ```
