# Xray Vmess

## 概述

用于在 Heroku 上部署 vmess+websocket+tls，每次部署自动选择最新的 alpine linux 和 xray core。  

## 镜像

提醒：滥用可能导致账户被删除！！！

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/Tonkercke/XVMS)

## 注意

### 路径

WebSocket 路径改为自定义，这个路径名称相当于WS的密码，所以要尽可能的长，最大長度是30位，超出后會創建失敗而且不會提示錯誤。
注意：路径名称只能是数字大小字母和一部分符号组成，符号不可以包含 “ / \ : *?"<>| ”。 

### 端口

`端口` 为 `443` 。

### alterId

`alterId` 为 `0` 。

### UUID

`UUID` 默认为 `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` UUID需要自行更改。

'UUID' 可在 https://www.uuidgenerator.net/ 中獲取。

## 流量中转

可以使用cloudflare的workers来`中转流量`，配置为：  

1.适用单一应用的反代代码

```
addEventListener(
  "fetch", event => {
    let url = new URL(event.request.url);
    url.host = "appname.herokuapp.com";
    let request = new Request(url, event.request);
    event.respondWith(
      fetch(request)
    )
  }
)
```

2.适用单双日循环执行的反代代码

```
const SingleDay = '应用程序名1.herokuapp.com'
const DoubleDay = '应用程序名2.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```

3.适用多实例循环执行的反代代码

```
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDate() % 5;
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4){
            host = Day4
        } else {
            host = Day1
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
