### 从服务器下载文件
```bash
scp <username>@<servername>:</remote_path/filename> <~/local_destination>
```

### 上传本地文件到服务器
```bash
scp <~/local_path/local_filename> <username>@<servername>:</remote_path>
```

### 从服务器下载整个目录
```bash
scp -r <username>@<servername>:</remote_path/remote_dir/> <~/local_destination>
```

### 上传目录到服务器
```bash
scp  -r <~/local_dir> <username>@<servername>:</remote_path/remote_dir>
```