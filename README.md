# Kitsunebi

A fully-featured V2Ray client for Android.

## 下载

<a href="https://play.google.com/store/apps/details?id=fun.kitsunebi.kitsunebi4android"><img src="https://play.google.com/intl/en_us/badges/images/generic/en-play-badge.png" height="100"></a>

Github Releases: https://github.com/eycorsican/Kitsunebi4Android/releases

## 扩展功能
扩展了 v2ray-core 的功能：
- 新增 Latency Balancing Strategy：[详情及配置看这里](https://gist.github.com/eycorsican/356debc8295e752c1df6ad7286f98ad4)

## 使用提示

- 配置文件可使用一个常见的 V2Ray JSON 配置
- App 使用较新的 v2ray-core 版本，你或许需要确保服务端也升级到相应的版本，具体版本号请看 Release Notes
- 把配置文件复制粘贴至主界面后，点击连接按钮即可启动
- 配置文件的 freedom outbound 推荐使用 [`UseIP` 策略](https://www.v2ray.com/chapter_02/protocols/freedom.html#outboundconfigurationobject)
- 配置文件不需要有 inbound，app 使用了 `tun2socks` 作为 inbound，默认开启 [http,tls 流量嗅探](https://www.v2ray.com/chapter_02/01_overview.html#sniffingobject) ，默认的标识（tag）为 "tun2socks"
- 关于 DNS 污染等问题，可以通过配置 V2Ray 的 DNS outbound 来解决，可在路由里添加一条识别出来自 tun2socks 的 DNS 流量，并转给 DNS outbound，具体请参考下面配置例子
- 下面是一个可以拿来日常使用的配置模板，在 `outbounds` 中替换上你的服务器信息即可，其中的路由规则和 DNS 规则从这个 [规则集文件](https://github.com/eycorsican/rule-sets/blob/master/kitsunebi_default.conf) 生成：
```json
{
    "dns": {
        "clientIp": "115.239.211.92",
        "hosts": {
            "localhost": "127.0.0.1"
        },
        "servers": [
            "114.114.114.114",
            {
                "address": "8.8.8.8",
                "domains": [
                    "google",
                    "android",
                    "fbcdn",
                    "facebook",
                    "domain:fb.com",
                    "instagram",
                    "whatsapp",
                    "akamai",
                    "domain:line-scdn.net",
                    "domain:line.me",
                    "domain:naver.jp"
                ],
                "port": 53
            }
        ]
    },
    "log": {
        "loglevel": "warning"
    },
    "outbounds": [
        {
            "protocol": "vmess",
            "settings": {
                "vnext": [
                    {
                        "address": "1.2.3.4",
                        "port": 10086,
                        "users": [
                            {
                                "id": "0e8575fb-a71f-455b-877f-b74e19d3f495"
                            }
                        ]
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp"
            },
            "tag": "proxy"
        },
        {
            "protocol": "freedom",
            "settings": {
                "domainStrategy": "UseIP"
            },
            "streamSettings": {},
            "tag": "direct"
        },
        {
            "protocol": "blackhole",
            "settings": {},
            "tag": "block"
        },
	{
	    "protocol": "dns",
	    "tag": "dns-out"
	}
    ],
    "policy": {
        "levels": {
            "0": {
                "bufferSize": 4096,
                "connIdle": 30,
                "downlinkOnly": 0,
                "handshake": 4,
                "uplinkOnly": 0
            }
        }
    },
    "routing": {
        "domainStrategy": "IPIfNonMatch",
        "rules": [
            {
				"inboundTag": ["tun2socks"],
                "network": "udp",
                "port": 53,
                "outboundTag": "dns-out",
                "type": "field"
            },
            {
                "domain": [
                    "domain:setup.icloud.com"
                ],
                "outboundTag": "proxy",
                "type": "field"
            },
            {
                "ip": [
                    "8.8.8.8/32",
                    "8.8.4.4/32",
                    "1.1.1.1/32",
                    "1.0.0.1/32",
                    "9.9.9.9/32",
                    "149.112.112.112/32",
                    "208.67.222.222/32",
                    "208.67.220.220/32"
                ],
                "outboundTag": "proxy",
                "type": "field"
            },
            {
                "ip": [
                    "geoip:cn",
                    "geoip:private"
                ],
                "outboundTag": "direct",
                "type": "field"
            },
            {
                "outboundTag": "direct",
                "port": "123",
                "type": "field"
            },
            {
                "domain": [
                    "domain:pstatp.com",
                    "domain:snssdk.com",
                    "domain:toutiao.com",
                    "domain:ixigua.com",
                    "domain:apple.com",
                    "domain:crashlytics.com",
                    "domain:icloud.com",
                    "cctv",
                    "umeng",
                    "domain:weico.cc",
                    "domain:jd.com",
                    "domain:360buy.com",
                    "domain:360buyimg.com",
                    "domain:douyu.tv",
                    "domain:douyu.com",
                    "domain:douyucdn.cn",
                    "geosite:cn"
                ],
                "outboundTag": "direct",
                "type": "field"
            },
            {
                "ip": [
                    "149.154.167.0/24",
                    "149.154.175.0/24",
                    "91.108.56.0/24",
                    "125.209.222.0/24"
                ],
                "outboundTag": "proxy",
                "type": "field"
            },
            {
                "domain": [
                    "twitter",
                    "domain:twimg.com",
                    "domain:t.co",
                    "google",
                    "domain:ggpht.com",
                    "domain:gstatic.com",
                    "domain:youtube.com",
                    "domain:ytimg.com",
                    "pixiv",
                    "domain:pximg.net",
                    "tumblr",
                    "instagram",
                    "domain:line-scdn.net",
                    "domain:line.me",
                    "domain:naver.jp",
                    "domain:facebook.com",
                    "domain:fbcdn.net",
                    "pinterest",
                    "github",
                    "dropbox",
                    "netflix",
                    "domain:medium.com",
                    "domain:fivecdm.com"
                ],
                "outboundTag": "proxy",
                "type": "field"
            }
        ],
        "strategy": "rules"
    }
}
```

## 开发相关问题

这个 V2Ray Android 客户端利用 `go-tun2socks` 把所有的 TCP/UDP 流量转给到 V2Ray 处理，所用的 `v2ray-core` 是没经过任何修改的官方版本，所以在配置和体验方面不会有太大差别。但在 Android 上有一些东西需要特殊处理，这也是在配置和使用上造成一些差别的地方。

- 在 Android 上有几个情况会造成流量/请求的死循环，目前就我所知的有以下几个：
  - 通过 VpnService 的 TUN 接口读取数据，再由代理程序代为发出时，如果不 [protect](https://developer.android.com/reference/android/net/VpnService#protect(int)) 代理程序用来发数据的 socket fd，代理程序发出的数据又会被转到 TUN 接口上，这个问题基本可以用 v2ray-core 提供的 `RegisterDialerController()` 和 `RegisterListenerController()` 两个接口完美解决
  - 还有一些由 Go 的 [net.DefaultResolver](https://golang.org/src/net/lookup.go#L102) 所发出的 DNS 查询流量没有被 protect 起来，在一些特定的情况下也有可能引起死循环
