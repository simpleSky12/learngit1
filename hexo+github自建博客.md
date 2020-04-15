# hexo+github自建博客



## 安装基本环境

[hexo中文官网](https://hexo.io/zh-cn/)



```bash
npm add hexo-cli -g  //全局安装 hexo

hexo init blog  //新建 blog 文件夹

cd blog

npm install // 安装基本依赖

hexo serve //启动服务
```



## 部署所需环境配置

安装 `hexo-deployer-git`

[hexo-deployer-git的github地址](https://github.com/hexojs/hexo-deployer-git)

```bash
npm install hexo-deployer-git --save
```

修改配置文件 **_config.yml**

```yaml
deploy:
  type: git
  repo: <repository url> #https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: [branch]
  message: [message]
  token: ''
  name: [git user]
  email: [git email]
  extend_dirs: [extend directory]
  ignore_hidden: false # default is true
  ignore_pattern: regexp  # whatever file that matches the regexp will be ignored when deploying
```

| 参数     | 描述                 | 默认                                                         |
| :------- | :------------------- | :----------------------------------------------------------- |
| `repo`   | 库（Repository）地址 | 常用ssh 地址，可以免密上传                                   |
| `branch` | 分支名称             | `gh-pages` (GitHub) `coding-pages` (Coding.net) `master` (others) |
| name     | github 用户名        |                                                              |
| email    | github 的登陆邮箱    |                                                              |

## github 仓库设置

![image-20200413145357170](http://qiniu.simplesky12.cn/img/image-20200413145357170.png)

新建的 github 仓库名称，要 以.github.io 结尾



创建仓库后，复制 ssh 链接

![image-20200413145557566](http://qiniu.simplesky12.cn/img/image-20200413145557566.png)



将ssh链接 配置到 delopy下的 repo 中

### 创建密钥对

因为使用了ssh，所以需要用到密钥，创建密钥对

```bash
ssh-keygen -t rsa -b 4096 -C "905781330@qq.com"
```

密钥生成后 将密钥配置到 github中，这样本地就可以使用ssh免密上传文件到 github中



配置完成后的文件内容

```yaml
deploy:
  type: git
  repo: git@github.com:simpleSky12/simpleSky12.github.io.git
  branch: master
  name: simpleSky12
  email: 905781330@qq.com
```



```bash
hexo g
```

产生静态文件目录 public

![image-20200413151105032](http://qiniu.simplesky12.cn/img/image-20200413151105032.png)



执行命令，部署静态文件到 github 上

```bash
hexo d
```



## 解决图片无法显示

安装新的插件 `hexo-asset-image`

```bash
npm istall hexo-asset-image --save
```



配置`_config.yml`里面的`post_asset_folder:false`这个选项设置为`true`。



## 博客主题的使用

[hexo的所有主题地址](https://hexo.io/themes/)

![image-20200413181451724](http://qiniu.simplesky12.cn/img/image-20200413181451724.png)



我喜欢的一款主题 [岛](https://shen-yu.gitee.io/)

[使用说明文档](https://shen-yu.gitee.io/2019/ayer/)

