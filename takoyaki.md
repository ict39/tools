# 章魚燒計畫
[簡報參考](https://docs.google.com/presentation/d/1nlNnABwdUEvX3TW9dYmmgEJf6in98Rot/edit?usp=sharing&ouid=113752483856941923298&rtpof=true&sd=true)  
~~專案名稱應景魷魚遊戲~~  
## 目的：
透過此計劃確認操作者使用我的機器要有基本的職能  
不同的帳號有不同的身份：資料科學家、網路工程師
資料科學家、網路工程師使用的命令不同，應受的考驗也不同。
一旦回答錯誤，CONTAINER 直接報廢，所有指令不可使用。


---
## 功能：  
1.資料容量大小計算  
2.網路代號計算  
3.檔案目錄權限判讀  

---
## 下載光碟片  
```
$ docker pull ict39/takoyaki
```

---
## 程式碼：  
```
$ cat sh
```
```bash=
#!/bin/sh
echo '5 second to answer question'
echo 'ready~'
sleep 2
echo 'Go!!!!!'
read -t 5 -p '57+42/6=?' ans
if [ "$ans" = "64" ]
then
  rm `which $0`
  sh
else
  echo "gg"
  rm `which busybox`
fi
```
---
```
$ cat ping
```
```bash=
#!/bin/sh
echo '20 second to answer question'
echo 'ready~'
sleep 2
echo 'Go!!!!!'
read -t 20 -p '192.168.30.127/26 ,network id is? ' ans
if [ "$ans" = "192.168.30.64" ]
then
  rm /usr/local/bin/ping /usr/local/bin/ifconfig /usr/local/bin/nc /usr/local/bin/curl &>/dev/null
else
  rm `which busybox`
  echo 'gg'
fi
```
---
```
$ cat mkdir
```
```bash=
#!/bin/sh
echo '10 second to answer question'
echo 'ready~'
sleep 2
echo 'Go!!!!!'
read -t 10 -p 'rwsr--r-x =? (number)' ans
if [ "$ans" = "4745" ]
then
  rm /usr/local/bin/mkdir /usr/local/bin/touch &>/dev/null
else
  rm `which busybox`
  echo 'gg'
fi
```
---
```
$ cat Dockerfile
```
```dockerfile=
FROM ict39/alpine

ADD sh ping mkdir /usr/local/bin/
RUN cp /usr/local/bin/ping /usr/local/bin/curl && \
    cp /usr/local/bin/ping /usr/local/bin/ifconfig && \
    cp /usr/local/bin/ping /usr/local/bin/nc && \
    cp /usr/local/bin/mkdir /usr/local/bin/touch
ENTRYPOINT ["sh"]
```
---
```
$ docker build —no-cache -t ict39/takoyaki .

```

###### tags: `職能`
