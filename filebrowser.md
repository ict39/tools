# File Browser

若使用 alpine.sshd 這片光碟片，需要先消磁  
```
$ cat Dockerfile
```
```dockerfile=
FROM quay.io/ict39/alpine
RUN apk update &&  apk add bash curl && apk upgrade && \
 curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash && \
 mkdir /opt/fbs
ENTRYPOINT ["filebrowser", "-a", "0.0.0.0", "-p", "8080", "-r", "/opt/fbs"]
```

docker build 燒成光碟片  
```
$ docker build --no-cache -t quay.io/ict39/alpine.fbs .
```

建立 Host Data Volume 的目錄  
```
$ sudo mkdir /opt/fbs
```

要指定 port number、volume  
```
$ docker run -itd --name fbs -p 8080:8080 -v /opt/fbs:/opt/fbs quay.io/ict39/alpine.fbs
```

---

以下K8S 的 port forward無法成功  

```bash=
#!/bin/sh
gw=$(route -n | grep -e "^0.0.0.0 ")
export GWIF=${gw##* }
ips=$(ifconfig $GWIF | grep 'inet ')
export IP=$(echo $ips | cut -d' ' -f2 | cut -d':' -f2)
filebrowser -a $IP -p 8080 -r /opt/fbs
```

```
$ cat Dockerfile
```
```dockerfile=
FROM quay.io/ict39/alpine
ADD 
RUN apk update &&  apk add bash curl && apk upgrade && \
 curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash && \
 mkdir /opt/fbs
ENTRYPOINT [start_fbs]
```





-a 參數
filebrowser 能監聽的 IP 只有三個 localhost、自己 IP、0.0.0.0  
監聽的是封包的 destination  
之前在 docker 練習時，能夠連到服務，是因為 docker 有 DNAT 功能  
而我們目前 K8S 的 port forward 並沒有 DNAT  
所以封包的 destination 是 192.168.x.4  
並不是 10.233.x.x  
所以 filebrowser 不會理這些封包  
要將 -a 後面改成 0.0.0.0  




###### tags: `職能`
