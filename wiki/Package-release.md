# 打包 & 发布

构建生成的资源文件保存在```/dist```目录下，可通过```config/index.js```目录文件修改相关配置信息

```bash
# 构建生产环境(默认)
npm run build

# 构建测试环境
npm run build --qa

# 构建验收环境
npm run build --uat

# 构建生产环境
npm run build --prod
```