### 安装ngrok
需要golang环境
```bash
# 进入ngrok要存放的目录
cd /usr/local/

git clone https://github.com/inconshreveable/ngrok.git

# 配置变量（临时）
export GOPATH=/usr/local/ngrok/
export NGROK_DOMAIN="ngrok.oigo.club" # ngrok.oigo.club是要映射的域名

cd ngrok
# 给当前配置的域名生成证书
openssl genrsa -out rootCA.key 2048  
openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pem  
openssl genrsa -out server.key 2048  
openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr  
openssl x509 -req -in server.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.crt -days 5000

# 替换证书文件,输入y即可
cp rootCA.pem assets/client/tls/ngrokroot.crt  
cp server.crt assets/server/tls/snakeoil.crt  
cp server.key assets/server/tls/snakeoil.key  

# 改配置文件(如果是在天朝的服务器需要改，香港或者国外的服务器不需要)
vim /usr/local/ngrok/src/ngrok/log/logger.go  
log "github.com/keepeye/log4go"  

```

## 配置服务端(go env查看自己的服务器操作系统)
centos7 64位配置如下:
```bash
cd ~/.golang/go/src # ~/.golang是go的存放目录
GOOS=linux GOARCH=amd64 ./make.bash  
cd /usr/local/ngrok/  
GOOS=linux GOARCH=amd64 make release-server  
```

## 配置客户端
mac 64位配置如下：
```bash
cd ~/.golang/go/src # ~/.golang是go的存放目录
GOOS=darwin GOARCH=amd64 ./make.bash
cd /usr/local/ngrok/
GOOS=darwin GOARCH=amd64 make release-client
```
将客户端执行文件目录(`/usr/local/ngrok/bin/darwin_amd64`)下载到本地, 在`darwin_amd64/`目录添加配置文件`ngrok.cfg`内容如下：
```bash
server_addr: "ngrok.oigo.club:4443"
trust_host_root_certs: false
```
修改`darwin_amd64/`目录权限
```bash
sudo chmod 777 ../darwin_amd64/* 
```


## 启动服务端
```bash
/usr/local/ngrok/bin/ngrokd -domain="$NGROK_DOMAIN" -httpAddr=":80" -httpsAddr=":8080"
```
`httpAddr`、`httpsAddr` 分别是 ngrok 用来转发`http`、`https`服务的端口，可以随意指定。`ngrokd` 还会开一个 `4443` 端口用来跟客户端通讯（可通过 `-tunnelAddr=":xxx"` 指定），如果你配置了 `iptables` 规则，需要放行这三个端口上的 TCP 协议

## 启动客户端
```bash
# 指定子域为abc，转发本地7777端口的服务
./ngrok -config=./ngrok.cfg -subdomain abc 7777
```

**参考**：
- [自己服务器搭建NGROK](https://www.jianshu.com/p/406b041a7635)
- [搭建内网穿透服务Ngrok](https://zhuanlan.zhihu.com/p/25768758?utm_source=wechat_session&utm_medium=social)