# 介绍

fast-vue基于vue、element-ui构建开发，实现fast-boot后台管理前端功能，提供一套更优的前端解决方案。

# 功能

```
- 登录/退出 (接口数据交互)
- 管理员列表 (接口数据交互)
- 角色管理 (接口数据交互)
- 菜单管理 (接口数据交互)
- SQL监控 (接口数据交互)
- 定时任务 (接口数据交互)
- 参数管理 (接口数据交互)
- 文件上传 (接口数据交互)
- 系统日志 (接口数据交互)
- 前后端分离，通过token进行数据交互，可独立部署
- 主题定制，通过scss变量统一一站式定制
- 动态菜单，通过菜单管理统一管理访问路由
- 数据切换，通过mock配置对接口数据／mock模拟数据进行切换
- 发布时，可动态配置CDN静态资源／切换新旧版本
- 更多，持续迭代中...
```

# 技术栈

你需要在本地安装 nodejs，提前了解和学习这些知识会对使用本项目有很大的帮助。

- [nodejs](http://nodejs.org/)
- [ES6](http://es6.ruanyifeng.com/)
- [vue-cli](https://github.com/vuejs/vue-cli)
- [vue](https://cn.vuejs.org/index.html)
- [vue-router](https://github.com/vuejs/vue-router)
- [vuex](https://github.com/vuejs/vuex)
- [axios](https://github.com/axios/axios)
- [vue-cookie](https://github.com/alfhen/vue-cookie)
- [element-ui](https://github.com/ElemeFE/element)
- [iconfont](http://www.iconfont.cn/)

# 目录结构

本项目已经通过vue-cli脚手架为你生产完整的开发框架（有根据业务需求做调整修改），下面是整个项目的目录结构。

```bash
├── dist                       // 构建打包生成部署文件
│   ├── 2007131111             // 静态资源
│   ├── config                 // 配置
│   ├── index.html             // index.html入口
├── build                      // 构建相关  
├── config                     // 构建配置相关
├── src                        // 源代码
│   ├── assets                 // 静态资源
│   ├── components             // 全局公用组件
│   ├── element-ui             // element-ui组件配置
│   ├── element-ui-theme       // element-ui组件主题配置
│   ├── icons                  // 所有 svg icons
│   ├── mock                   // mock 模拟数据
│   ├── router                 // 路由
│   ├── store                  // 全局 store管理
│   ├── utils                  // 全局公用方法
│   ├── views                  // view
│   ├── App.vue                // 入口组件
│   ├── main.js                // 入口
├── static                     // 第三方不打包资源
│   ├── config                 // 全局变量配置
│   ├── plugins                // 插件
├── .babelrc                   // babel-loader 配置
├── eslintrc.js                // eslint 配置项
├── .gitignore                 // git 忽略项
├── favicon.ico                // favicon图标
├── index.html                 // html模板
└── package.json               // package.json
```

# 安装

安装过程中，可能会出现安装过慢、报错等情况，请尝试以下2种方式：

```bash
# 克隆项目
git clone ${fast-vue.git}

# 安装依赖
# 1
npm install -g cnpm --registry=https://registry.npm.taobao.org
# 2
cnpm install

# 启动服务
npm run dev
```

```bash
# 克隆项目
git clone ${fast-vue.git}

# 安装依赖
npm install

# 启动服务
npm run dev
```

启动完成后会自动打开浏览器访问 [http://localhost:8001]()。
