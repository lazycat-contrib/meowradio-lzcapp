# 阿喵收音机 · meowRadio

[上游项目](https://github.com/ca-x/meowRadio)的懒猫微服应用包。支持全球电台搜索、个人收藏、多种播放器主题与睡眠定时。

需要懒猫系统 1.5.0+，应用镜像目标为 Linux amd64。图标使用用户提供的本地图片。

安装时通过设置向导填写账号和密码，两者默认随机生成。打开首页登录入口进入 `/login` 后自动填充；仅在懒猫已认证用户访问时注入。此账号是应用管理员，获准访问本应用的用户可通过自动填充使用该账号。初始化只执行一次，更改部署参数不会重置数据库中的账号密码。

数据库与收藏保存到 `/lzcapp/var/data`，映射容器 `/data`。服务以容器用户 `0:0` 运行，使用普通目录挂载，避免部分系统上 `run_as` 的 idmapped 挂载创建失败。HTTPS 会话启用 Secure Cookie。

电台目录由 Radio Browser 提供。音源可能离线或有地区限制；HTTPS 页面播放 HTTP 音源可选择服务端中转。

```sh
lzc-cli project release -o dist/meowradio.lpk
lzc-cli lpk info dist/meowradio.lpk
```

GitHub Actions 每日检查上游稳定版本，将镜像复制到懒猫镜像仓库，构建 LPK，创建版本化 GitHub Release 并发布到私有商店与官方商店。官方发布需等待审核。相同或更新的商店版本会跳过，禁止自动降级。

工作流使用仓库已获授权的组织 Secrets：`LZC_API_TOKEN`、`APPSTORE_URL`、`APPSTORE_TOKEN`、`PRIVATE_STORE_GROUP_CODES`。仓库未设置覆盖值。

Release 文件命名为 `community.lazycat.app.meowradio-v<version>.lpk`；私有商店使用该 Release 地址和 SHA256，官方商店上传同一份已校验文件。

商店截图取自上游 v0.1.1 的 `docs/screenshots/pc-home.png` 和 `pc-timer.png`。
