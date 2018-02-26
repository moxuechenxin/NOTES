## phantomjs
- `PhantomJS not found on PATH`
1. **下载phantomjs**  
如果下载失败
```bash
# 在~/.npmrc添加如下内容
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