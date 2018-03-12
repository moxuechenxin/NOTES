### 1. 检查swap大小和硬盘空间

```bash
free -m
```

### 2. 添加交换文件
```bash
mkdir -p /opt/temp
# 此处增加2G
dd if=/dev/zero of=/opt/temp/swap bs=1024 count=2048000
```

### 3. 创建交换空间
```bash
mkswap /opt/temp/swap
```

### 4. 启动新增加的交换空间
```bash
swapon /opt/temp/swap
free -m
```

### 5. 修改/etc/fstab
```bash
echo "/opt/temp/swap           swap                    swap    defaults        0 0" >>/etc/fstab
```

- - -
**参考**：
- [linux增大swap空间](https://blog.slogra.com/post-246.html)
- [Swap交换分区概念](https://www.cnblogs.com/kerrycode/p/5246383.html)