## 安装和本地预览
### 安装
```shell
npm install -g hexo
```
### 初始化项目
```shell
hexo init
```
### 生成静态页面
```shell
# 缩写：hexo g
hexo generate 
```
### 启动本地服务
```shell
# 缩写：hexo s
hexo server
```
## 部署
### github建立博客仓库
1. 仓库名必须是以下格式：  
`<your_user_name>.github.io`  

2. 修改`_config.yml`  

```
deploy:
     type: git
     repo: git@github.com:zhengjitf/zhengjitf.github.io
     branch: master
```

3.安装`hexo-deployer-git`

```shell
npm install hexo-deployer-git --save
```

### 部署步骤
1. `hexo clean`
2. `hexo g`
3. `hexo d`

## 常用命令
命令 | 简写 | 描述 
---|---|---
`hexo init` | | 初始化一个目录
`hexo new "postName"` | `hexo n "postName"` | 新建文章
`hexo new page "pageName"` | `hexo n page "pageName"` | 新建页面
`hexo clean` | | 清除生成的静态文件（public目录）
`hexo generate` | `hexo g` | 生成静态页面至public目录
`hexo server` | `hexo s` | 本地预览（默认4000端口）
`hexo deploy` | `hexo d` | 将.deploy目录部署到GitHub
`hexo help` | |  查看帮助
`hexo version` | | 查看Hexo的版本

**参考**：
- http://charsdavy.github.io/2016/05/31/build-blog-by-hexo/
- http://www.jianshu.com/p/465830080ea9
