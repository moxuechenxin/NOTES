## 生成SSH key
```bash
ssh-keygen -t rsa
```
指定rsa算法生成密钥，生成两个文件`id_rsa`(私钥)和`id_rsa.pub`(公钥)

Linux/Mac系统在 `~/.ssh`下，win系统在`/c/Documents and Settings/<username>/.ssh` 下