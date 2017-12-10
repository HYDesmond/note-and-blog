# 前言
因为环境的问题折腾了好久，每次编译都失败，最后发现是版本的问题要用低版本的golong才行（神奇的错误），还有一个如果git版本过低会卡在一个地方，所以还要手工编译新的git
## 安装git2.6
+ #### 下载源码
git的下载地址：http://git-scm.com/downloads 或者[点击下载](https://www.kernel.org/pub/software/scm/git/git-2.6.0.tar.gz)

+ #### 安装编译环境
```
$ sudo apt-get installlibcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
```

+ #### 解压编译
```shell
$ tar zxvf git-2.6.0.tar.gz
$ cd git-2.6.0
$ ./configure --prefix=/usr/local/git
$ make
$ make install
```

+ ####创建软连接（这样可以直接使用不用带上绝对路径）
```
$ ln -s /usr/local/git/bin/* /usr/bin/
```

## 安装go环境
Ubuntu源里的编译会出错要自己编译，不知道是不是我自己电脑的原因

+ #### 下载源码
go的下载地址：http://www.golangtc.com/download 或者[百度云](http://pan.baidu.com/s/1pL0Ca4V)（版本太高可能会出错，不知道为什么）

+ ####解压安装
```
$ tar -zxvf go1.4.2.linux-386.tar.gz
$ mv go /usr/local/         //这个位置可以变但和后面的要对应
```

+ ####建立软链
```
$ ln -s /usr/local/go/bin/* /usr/bin/
```

## 编译ngrok

+ #### 下载源码配置
```
$ cd /usr/local/
$ git clone https://github.com/inconshreveable/ngrok.git
$ export GOPATH=/usr/local/ngrok/
$ export NGROK_DOMAIN="设置你的域名，比如ngrok.abc.com"
$ cd ngrok
```

+ #### 生成SSL证书
```
//要指定我们自己的服务器就要自己生成ssl证书
$ openssl genrsa -out rootCA.key 2048
$ openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pem
$ openssl genrsa -out server.key 2048
$ openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr
$ openssl x509 -req -in server.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.crt -days 5000
//复制到指定位置
$ cp rootCA.pem assets/client/tls/ngrokroot.crt
$ cp server.crt assets/server/tls/snakeoil.crt
$ cp server.key assets/server/tls/snakeoil.key
```
+ #### 编译服务端
````
$ cd /usr/local/go/src
$ GOOS=linux GOARCH=386 ./make.bash
$ cd /usr/local/ngrok/
$ GOOS=linux GOARCH=386 make release-server
````
+ #### 编译客户端
````
$ cd /usr/local/go/src
$ GOOS=linux GOARCH=amd64 ./make.bash
$ cd /usr/local/ngrok/
$ GOOS=linux GOARCH=adm64 make release-server
````

+ #### 对应版本的编译命令
```
# Windows64位下
GOOS=windows GOARCH=amd64 make release-server
GOOS=windows GOARCH=amd64 make release-client
# Windows32位下
GOOS=windows GOARCH=386 make release-server
GOOS=windows GOARCH=386 make release-client
# Liunx64位下
GOOS= liunx GOARCH=amd64 make release-server
GOOS= liunx GOARCH=amd64 make release-client
#  Liunx32位下
GOOS= liunx GOARCH=386 make release-server
GOOS= liunx GOARCH=386 make release-client
```

+ #### 启动服务端
首先先创建一个日志文件，方便将服务端输出的日志存起来，然后后台启动服务端，端口可以自己设置
```
# cd /usr/local/ngrok/bin/ngrokd
# touch ngrok.log
# ./ngrokd -domain="$NGROK_DOMAIN" -httpAddr=":80" >> ngrok.log &
```

+ #### 客户端创建配置文件
内容如下：
```
server_addr: "之前设置的域名:4443"
trust_host_root_certs: false
```

+ #### 客户端使用
ngrok.cfg就是上一步建立的文件
比如我们要用这个域名访问text.ngrok.abc.com
text是要使用的域名前缀，就是subdomain的值
80就是本地web server 的端口号
```
./ngrok -config=./ngrok.cfg -subdomain=text 80
```

