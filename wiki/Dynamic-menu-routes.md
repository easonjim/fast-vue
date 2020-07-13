# 动态菜单路由

通过菜单管理统一管理访问路由，自动映射对应文件目录[```src/views/modules/...```]组件。功能实现通过拦截路由进行逻辑判断处理。

#### 这里需特别注意，在通过左侧边栏菜单管理，操作新增／修改[菜单路由]项，进行自动映射对应文件目录组件时的规则，规则如下：

- 值为```/sys/user```自动映射对应文件目录为```src/views/modules/[sys/user.vue]```文件
- 值项为```/job/schedule```自动映射对应文件目录为```src/views/modules/[job/schedule.vue]```文件
- 值项为```http://fast.demo.domain.com/```自动使用iframe访问```http://fast.demo.domain.com/```地址，也就是说**只要是以http[s]?://开头时，都通过iframe访问地址**，不再自动映射


#### 如何确定新增／修改菜单路由是否映射成功？

所有的动态菜单路由都会保存在```sessionStorage['dynamicMenuRoutes']```属性中，同时输出在浏览器控制台
```
!<-------------------- 动态(菜单)路由 s -------------------->
数据对象
!<-------------------- 动态(菜单)路由 s -------------------->
```
1. 先查看```dynamicMenuRoutes```每一项中的```meta: { iframeUrl: '' }```属性，如果```iframeUrl === ''```，那么就会自动映射对应文件目录。如果```iframeUrl !== ''```就会通过iframe访问地址，无需查看第二步。

2. 再查看```dynamicMenuRoutes```每一项中的```component: null```属性，如果```component === null ```，那么就是自动映射对应文件目录```失败```。如果```component !== null```自动映射对应文件目录```成功```。


> 对应代码在 [src/router/index.js](../src/router/index.js)

```js
router.beforeEach((to, from, next) => {
  // 添加动态(菜单)路由
  // 1. 已经添加 or 全局路由, 直接访问
  // 2. 获取菜单列表, 添加并保存本地存储
  if (router.options.isAddDynamicMenuRoutes || fnCurrentRouteType(to) === 'global') {
    next()
  } else {
    http({
      url: http.adornUrl('/sys/menu/nav'),
      method: 'get',
      params: http.adornParams()
    }).then(({data}) => {
      if (data && data.code === 0) {
        fnAddDynamicMenuRoutes(data.menuList)
        router.options.isAddDynamicMenuRoutes = true
        sessionStorage.setItem('menuList', JSON.stringify(data.menuList || '[]'))
        sessionStorage.setItem('permissions', JSON.stringify(data.permissions || '[]'))
        next({ ...to, replace: true })
      } else {
        sessionStorage.setItem('menuList', '[]')
        sessionStorage.setItem('permissions', '[]')
        next()
      }
    })
  }
})

/**
 * 判断当前路由类型, global: 全局路由, main: 主入口路由
 * @param {*} route 当前路由
 */
function fnCurrentRouteType (route) {
  var temp = []
  for (var i = 0; i < globalRoutes.length; i++) {
    if (route.path === globalRoutes[i].path) {
      return 'global'
    } else if (globalRoutes[i].children && globalRoutes[i].children.length >= 1) {
      temp = temp.concat(globalRoutes[i].children)
    }
  }
  return temp.length >= 1 ? fnCurrentRouteType(route, temp) : 'main'
}

/**
 * 添加动态(菜单)路由
 * @param {*} menuList 菜单列表
 * @param {*} routes 递归创建的动态(菜单)路由
 */
function fnAddDynamicMenuRoutes (menuList = [], routes = []) {
  var temp = []
  for (var i = 0; i < menuList.length; i++) {
    if (menuList[i].list && menuList[i].list.length >= 1) {
      temp = temp.concat(menuList[i].list)
    } else if (/\S/.test(menuList[i].url)) {
      var route = {
        path: menuList[i].url.replace('/', '-'),
        component: null,
        name: menuList[i].url.replace('/', '-'),
        meta: {
          menuId: menuList[i].menuId,
          title: menuList[i].name,
          isDynamic: true,
          isTab: true,
          iframeUrl: ''
        }
      }
      // url以http[s]://开头, 通过iframe展示
      if (isURL(menuList[i].url)) {
        route['path'] = `i-${menuList[i].menuId}`
        route['name'] = `i-${menuList[i].menuId}`
        route['meta']['iframeUrl'] = menuList[i].url
      } else {
        try {
          route['component'] = _import(`modules/${menuList[i].url}`) || null
        } catch (e) {}
      }
      routes.push(route)
    }
  }
  if (temp.length >= 1) {
    fnAddDynamicMenuRoutes(temp, routes)
  } else {
    mainRoutes.name = 'main-dynamic'
    mainRoutes.children = routes
    router.addRoutes([
      mainRoutes,
      { path: '*', redirect: { name: '404' } }
    ])
    sessionStorage.setItem('dynamicMenuRoutes', JSON.stringify(mainRoutes.children || '[]'))
    console.log('\n%c!<-------------------- 动态(菜单)路由 s -------------------->', 'color:blue')
    console.log(mainRoutes.children)
    console.log('%c!<-------------------- 动态(菜单)路由 e -------------------->\n\n', 'color:blue')
  }
```