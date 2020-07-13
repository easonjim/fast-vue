# mock/api数据切换

在前后端分离的情况下，往往前后台是并行开发的！两队先讨论商议确定接口名称、请求类型、请求参数、返回数据等后，就可以暂时并行开发了。为了让前端更加灵活的、更快捷的进行业务逻辑开发，这里提供通过mockJs本地接口拦截，返回模拟数据功能。

#### 开启mock本地模拟数据

1. 通过设置[/src/mock/index.js)](../src/mock/index.js)文件中```fnCreate(common, [false])```[]中括号中为true／false开启关闭当前模块mock本地模拟数据功能。（默认开启）
2. 通过设置[/src/mock/modules/]()文件下模块```isOpen: [false]```[]中括号中为```true／false```开启关闭当前api接口mock本地模拟数据功能。（默认开启）

> 具体模拟数据请参考：[mockJs](https://github.com/nuysoft/Mock)