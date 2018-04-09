###  deploy报错： `bash: npm: command not found`  
**原因**：nvm安装node的环境问题  
**fix**：
在`.bashrc`文件添加如下脚本：
```bash
export NVM_DIR="/root/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm
```
[参考](https://github.com/Unitech/pm2-deploy/issues/41)