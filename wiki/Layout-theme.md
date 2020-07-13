# 布局 & 主题

整体布局包括：
 - 头部导航条 main-navbar
 - 左侧边栏 main-sidebar
 - 中间内容展示 main-content

> 对应代码在 [src/views/main.vue](../src/views/main.vue)

```fast-vue```中除去```404```、```login```页面，其它页面都是基于这个布局的。这里我将

- 无需嵌套上左右整体布局的路由称为“全局路由”放在```globalRoutes```常量中，

- 需嵌套上左右整体布局的路由称为“主入口路由”放在```mainRoutes```常量的```children```属性中。

> 对应代码在 [src/router/index.js](../src/router/index.js)

```js
// 全局路由(无需嵌套上左右整体布局)
const globalRoutes = [
  { path: '/404', component: _import('common/404'), name: '404', meta: { title: '404未找到' } },
  { path: '/login', component: _import('common/login'), name: 'login', meta: { title: '登录' } }
]

// 主入口路由(需嵌套上左右整体布局)
const mainRoutes = {
  path: '/',
  component: _import('main'),
  name: 'main',
  redirect: { name: 'home' },
  meta: { title: '主入口整体布局' },
  children: [
    // 通过meta对象设置路由展示方式
    // 1. isTab: 是否通过tab展示内容, true: 是, false: 否
    // 2. iframeUrl: 是否通过iframe嵌套展示内容, '以http[s]://开头': 是, '': 否
    { path: '/home', component: _import('common/home'), name: 'home', meta: { title: '首页' } },
    { path: '/theme', component: _import('common/theme'), name: 'theme', meta: { title: '主题' } },
    {
      path: '/demo-01',
      component: null, // 如需要通过iframe嵌套展示内容, 但不通过tab打开, 请自行创建组件使用iframe处理!
      name: 'demo-01',
      meta: {
        title: '我是一个通过iframe嵌套展示内容, 并通过tab打开 demo',
        isTab: true,
        iframeUrl: 'http://fast.demo.domain.com/'
      }
    }
  ],
  beforeEnter (to, from, next) {
    let token = Vue.cookie.get('token')
    if (!token || !/\S/.test(token)) {
      next({ name: 'login' })
    }
    next()
  }
}

const router = new Router({
  mode: 'hash',
  scrollBehavior: () => ({ y: 0 }),
  isAddDynamicMenuRoutes: false, // 是否已经添加动态(菜单)路由
  routes: globalRoutes.concat(mainRoutes)
})
```

同时，主入口路由提供meta对象2个属性设置路由展示方式。

```isTab```: 是否通过tab展示内容

```iframeUrl```: 是否通过iframe嵌套展示内容

> 如需要通过iframe嵌套展示内容, 但不通过tab打开, 请自行创建组件使用iframe处理!


# 菜单路由

之前版本的菜单路由都需要手动在```router/index.js```文件中添加，这次新版本提供通过菜单管理统一管理访问路由，自动映射对应文件目录组件。具体请点击移步[动态菜单路由](Dynamic-menu-routes.md)。


# 主题定制

提供12套颜色主题，进行element-ui和整站主题切换。具体切换方法如下：

1. 修改[/src/element-ui-theme/index.js](../src/element-ui-theme/index.js)文件中```import './element-[#17b3a3]/index.css'```[]中括号中的值，值可以在同文件中```list```属性中取即可。**（注意：这里只是修改element-ui组件主题）**
2. 修改[/src/assets/scss/_variables.scss](../src/assets/scss/_variables.scss)文件中```$--color-primary: [#17b3a3];```[]中括号中的值，值与第一步值同步即可。**（注意：这里只是修改站点主题，不包括element-ui组件主题）**

主题定制具体实现方法是：

1. 先通过element-ui官方提供的[在线主题生成工具](https://elementui.github.io/theme-chalk-preview/#/zh-CN)，进行切换主题色，再下载解压文件（保留```fonts目录中文件和index.css即可```）放置```/src/element-ui-theme/```目录中，使用同目录中的```index.js```进行统一配置管理。
2. 再设置修改站点主题，使整站主题色统一一致。

> 之所以放弃之前直接在项目中通过```scss```一站式设置主题，是因为开发时编译太慢！同时按需求加载组件的css文件并没有比现在的```index.css```文件小多少kb（主要大小还是在js文件上）。考虑到大部分一个站配置一个主题色后，不会频繁修改！

> 网上查阅也有通过打包插件，对```css```添加父级```id or class```进行主题切换的，但是这样就相当于12套皮肤，需要将12个```index.css```进行合并打包发布，那文件岂不是太大了！这样的需求暂不考虑！
