## phantomjs
- `PhantomJS not found on PATH`
1. **下载phantomjs**  
如果下载失败
```bash
# 在~/.npmrc添加如下内容
sass_binary_site = https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl = https://npm.taobao.org/mirrors/phantomjs/
chromedriver_cdnurl=https://npm.taobao.org/mirrors/chromedriver
```
2. **解压**
```bash
# 如果解压报错，需要安装bz2：yum install bz2
tar -xjvf phantomjs-2.1.1-linux-x86_64.tar.bz2 
``` 
3. **创建软链接**
```bash
ln -s <path/to/phantomjs> /usr/bin
```
4. **测试**
```bash
phantomjs –version
```

- `phantomjs: error while loading shared libraries: libfreetype.so.6: cannot open shared object file: No such file or directory`
```bash
sudo yum install freetype
```
-  `phantomjs: error while loading shared libraries: libfontconfig.so.1: cannot open shared object file: No such file or directory`
```bash
sudo yum install fontconfig
```

## node-sass
设置二进制文件淘宝镜像
```bash
# 或者在~/.bash_profile中添加`export SASS_BINARY_SITE=http://npm.taobao.org/mirrors/node-sass`
SASS_BINARY_SITE=http://npm.taobao.org/mirrors/node-sass npm install node-sass
```
系统环境变化需要重新编译二进制文件
```bash
npm rebuild node-sass --force
```
