欢迎来到 fast-vue Wiki!

* **[开始 & 安装](Getting-started.md)**
* **[布局 & 主题](Layout-theme.md)**
* **[动态菜单路由](Dynamic-menu-routes.md)**
* **[mock/api数据切换](Data.md)**
* **[打包 & 发布](Package-release.md)**

# 常见问题

### 开发时，如何连接后台项目api接口？
修改```/static/config/index.js```目录文件中``` window.SITE_CONFIG['baseUrl'] = '本地api接口请求地址';```

&nbsp;

### 开发时，如何解决跨域？
1. 修改```/config/dev.env.js```目录文件中```OPEN_PROXY: true```开启代理
2. 修改```/config/index.js```目录文件中```proxyTable```对象```target: '代理api接口请求地址'```
3. 重启本地服务

&nbsp;

### 开发时，如何提前配置CDN静态资源？
修改```/static/config/index-[qa/uat/prod].js```目录文件中```window.SITE_CONFIG['domain']  = '静态资源cdn地址';```

&nbsp;

### 构建生成后，发布需要上传哪些文件？
```/dist```目录下：```2007131111、config（配置文件）、index.html```

&nbsp;

### 构建生成后，如何动态配置CDN静态资源？
修改```/dist/config/index.js```目录文件中```window.SITE_CONFIG['domain']  = '静态资源cdn地址';```

&nbsp;

### 构建生成后，如何动态切换新旧版本？
修改```/dist/config/index.js```目录文件中``` window.SITE_CONFIG['version'] = '旧版本号';```