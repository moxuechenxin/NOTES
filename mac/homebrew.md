# homebrew
## 安装
```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" 
```
#### 添加环境变量
```bash
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
```
#### 检测homebrew是否能正常运行
```bash
brew doctor
```

### homebrew基本使用
#### 搜索包
```bash
brew search <keyword/regex>
```

#### 安装一个包
```bash
brew install <package_name>
```

#### 卸载
```bash
# 使用--force可以卸载指定包的全部版本
brew uninstall <package_name>
```

#### 切换包的不同版本
```bash
brew switch <包名> <版本号>
```

#### 更新 Homebrew 在服务器端上的包目录
```bash
brew update
```

#### 查看你的包是否需要更新
```bash
brew outdated <package_name>
```

#### 更新包
```bash
brew upgrade <package_name>
```

#### 清理旧版本的包缓存
```bash
brew cleanup <package_name>
```

#### 查看你安装过的包列表（包括版本号）
```bash
brew list --versions
```

## 扩展tap
#### 安装第三方的仓库
```bash
#添加或者删除仓库
brew [un]tap <github_userid/repo_name>
```

#### 列出当前已经tapped 的仓库
```bash
brew tap
```

## Homebrew Cask
安装图形化程序
```bash
brew tap caskroom/cask  # 添加 Github 上的 caskroom/cask 库
brew cask install google-chrome # 安装 Google 浏览器
```

**参考**：
- [Homebrew总结](https://zhuanlan.zhihu.com/p/22598799)
- [homebrew的tap功能详解](https://segmentfault.com/a/1190000012826983)
- [Homebrew极客学院](http://wiki.jikexueyuan.com/project/mac-dev-setup/homebrew.html)