* [下载 mjpg-streamer-r63.tar.gz](https://sourceforge.net/projects/mjpg-streamer/)

* 安装 libjpeg-dev 库,并编译得到库文件

```shell
yum install libjpeg*
tar -zxvf mjpg-streamer-r63.tar.gz
cd mjpg-streamer-r63
make
```

* 启动服务

```shell
./mjpg_streamer -o "output_http.so -w ./www -p 8879"
```

* web 访问

```
http://127.0.0.1:8879/?action=stream
```
