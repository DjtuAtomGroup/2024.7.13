# 代理
* 参考[博客](https://www.junz.org/post/v2_in_linux/)和[白话文教程](https://toutyrater.github.io/)
## Windows Proxy
Windows下的代理软件我们选择V2rayN
V2rayN是windows下V2ray的客户端程序
由于其是图形界面，且其不用更改配置文件，因此是比较容易的
1. 下载[V2rayN](https://github.com/2dust/v2rayN)
2. 登入机场一键复制订阅地址
3. 打开v2rayN找到订阅, 打开订阅设置, 将复制内容粘贴到V2rayN中即可
## Linux Proxy
Linux下我们配置代理有两种方式
1. v2rayA
2. 直接使用v2ray内核
V2rayA是Linux下V2ray的客户端程序(web)
由于是图形界面，且其不用更改配置文件。因此相对于手动配置代理是比较容易的
### [V2rayA](https://github.com/v2rayA/v2rayA)
使用V2rayA需要手动下载内核(只需内核)
[V2rayA的官方文档](https://v2raya.org/en/)
对于Arch用户，安装VrayA只需用yay安装即可，v2ray内核可以需要通过github获取
### 直接使用v2ray内核
{% note success %}
不建议小白直接使用内核，想折腾的除外
{% endnote %}
注意配置文件的刷新问题，有时候不一定是配置错误了
* 下载[V2ray](https://github.com/v2ray/v2ray-core)中的发行版本(即releases)
* 解压后将相应文件放置到对应路径，没有的文件在相应位置新建即可
  ```
  # 将以下文件手动放到指定位置，也可用自动化脚本，但注意审核第三方脚本内容
  # 以下文件注意权限问题
  v2ray -> /usr/local/bin/v2ray
  v2ctl -> /usr/local/bin/v2ctl
  geoip.dat -> /usr/local/share/v2ray/geoip.dat
  geosite.dat -> /usr/local/share/v2ray/geosite.dat
  config.json -> /usr/local/etc/v2ray/config.json
  access.log -> /var/log/v2ray/access.log
  error.log -> /var/log/v2ray/error.log
  v2ray.service -> /etc/systemd/system/v2ray.service
  v2ray@.service -> /etc/systemd/system/v2ray@.service
  ```
* 配置config.json文件(可以将其他客户端的配置文件导出到这里使用)
  以下内容仅供参考(只需更改节点地址即可)
  ```  json
  {
    "log": {
      "access": "/var/log/v2ray/access.log",
      "error": "/var/log/v2ray/error.log",
      "loglevel": "warning"
    },
    "inbounds": [
      {
        "tag": "socks",
        "port": 10808,
        "listen": "127.0.0.1",
        "protocol": "socks",
        "sniffing": {
          "enabled": true,
          "destOverride": [
            "http",
            "tls"
          ]
        },
        "settings": {
          "auth": "noauth",
          "udp": true,
          "allowTransparent": false
        }
      },
      {
        "tag": "http",
        "port": 10809,
        "listen": "127.0.0.1",
        "protocol": "http",
        "sniffing": {
          "enabled": true,
          "destOverride": [
            "http",
            "tls"
          ]
        },
        "settings": {
          "auth": "noauth",
          "udp": true,
          "allowTransparent": false
        }
      }
    ],
    "outbounds": [
      {
        "tag": "proxy",
        "protocol": "vmess",
        "settings": {
          "vnext": [
            {
              "address": "节点地址",
              "port": 80,
              "users": [
                {
                  "id": "ab4c04e1-cc44-4f59-af3e-3a7584e3ad65",
                  "alterId": 0,
                  "email": "t@t.tt",
                  "security": "auto"
                }
              ]
            }
          ]
        },
        "streamSettings": {
          "network": "ws",
          "wsSettings": {
            "path": "/",
            "headers": {
              "Host": "tms.dingtalk.com"
            }
          }
        },
        "mux": {
          "enabled": false,
          "concurrency": -1
        }
      },
      {
        "tag": "direct",
        "protocol": "freedom",
        "settings": {}
      },
      {
        "tag": "block",
        "protocol": "blackhole",
        "settings": {
          "response": {
            "type": "http"
          }
        }
      }
    ],
    "routing": {
      "domainStrategy": "IPIfNonMatch",
      "rules": [
        {
          "type": "field",
          "inboundTag": [
            "api"
          ],
          "outboundTag": "api",
          "enabled": true
        },
        {
          "type": "field",
          "port": "0-65535",
          "outboundTag": "proxy",
          "enabled": true
        }
      ]
    }
  }
  ```
* 终端实现代理，添加环境变量到shell配置文件中
  ```
  export ALL_PROXY="socks5://127.0.0.1:10808"  
  export http_proxy="http://127.0.0.1:10809"  
  ```
* 浏览器使用代理需要在浏览器设置中进行设置
* 使用
  ```
  # 启动
  sudo systemctl start v2ray
  # 检查状态
  sudo systemctl status v2ray
  # 设置开机自启动
  sudo systemctl enable v2ray
  ```
* 测试`curl -x socks5://127.0.0.1:10808 https://www.google.com -v` 成功会输出google的html代码
* 使用proxychains强制走代理
  对于某些不会走socks5的应用，可以通过proxychains让其强制走代理
  在`/etc/proxychains4.conf` 中添加 `socks5 127.0.0.1 10808` ，并删除sock4那行即可
  之后即可在命令前加 `proxychains` 即可强制走代理
  如：`proxychains wget https://xxxxxx.com`

