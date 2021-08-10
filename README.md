# private-npm-storage

#### 安装

```
npm install -g verdaccio
```

全局安装插件之后，会在C:\Users\Administrator\AppData\Roaming\verdaccio\config.yaml生成一个.yaml配置文件。用来配置package发布、访问权限和一些基本的配置。

#### config.yaml配置说明

```yaml
# This is the default config file. It allows all users to do anything,
# so don't use it on production systems.

# 发布package存放路径
storage: ./storage
# 包含插件的目录的路径
plugins: ./plugins

# web配置
web:
  title: Verdaccio-npm
  # comment out to disable gravatar support
  gravatar: false
  # 排序规则 (asc|desc)
  sort_packages: asc
  # 暗黑主题
  darkMode: false

# 国际化配置
i18n:
   #支持语言列表 https://github.com/verdaccio/ui/tree/master/i18n/translations
  web: ['zh-CN']
   
# 登录认证
auth:
  htpasswd:
    # 账户存放路径
    file: ./htpasswd
    # 允许最大账户注册数量，最大数量为1000，设置为-1，则无法注册
    max_users: 10
# package发布代理列表，默认是npm    
uplinks:
  npmjs:
    url: https://registry.npmjs.org/

# package发布权限设置
# 权限分为三种。$all（所有人）、$anonymous（未注册用户）、$authenticated（注册用户）
# @*/*正则表达式匹配
packages:
  '@*/*':
    # scoped packages
    access: $authenticated #访问权限
    publish: $authenticated #发布权限
    unpublish: $authenticated #删除权限
    proxy: npmjs #代理设置，如果在私有库找不到，则会从proxy代理库中查找

  '**':
    access: $authenticated
    publish: $authenticated
    unpublish: $authenticated
    proxy: npmjs

# 私有库服务端相关的配置
server:
  keepAliveTimeout: 60

# 中间件相关配置，默认会引入 auit 中间件，来支持 npm audit 命令
middlewares:
  audit:
    enabled: true

# 日志设置
logs: { type: stdout, format: pretty, level: http }
```

#### 使用说明

```javascript
npm run start 在浏览器中访问 http://localhost:4873/就能访问私有仓库。如果访问权限设置不为all，那么需要用户登录后才能看到发布的package包。
npm run add:user 添加账户，如果发布权限设置(需要注册用户)，添加用户的htpasswd文件查看。
npm runpublish:package 发布package到私有仓库，发布的package可以在storage目录下查看。
npm set registry http://localhost:4873/ 设置npm代理为私有仓库地址。
```