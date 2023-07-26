# Sing-Box Configs

قصد دارم اینجا کانفیگ‌های مربوط به سینگ باکس رو بزارم رفرنس اصلی سینگ باکس سایت خودشون هست ..
```
https://sing-box.sagernet.org
```




برای اجرای سینگ باکس نیاز هست که یه فایل کانفیگ با فرمت json براش تعریف بشه توی که مشخص میکنه که چه پروتها و پروتکلهایی رو اجرا کنه .

# کلیات کانفیگ به شکل زیره :
```
{
  "log": {},
  "dns": {},
  "ntp": {},
  "inbounds": [],
  "outbounds": [],
  "route": {},
  "experimental": {}
}
```

من خودم برای نصب روی اوبنتو به شکل زیر میرم ، فقط باید لینک آخرین نسخه رو جایگزین کنید !

```
wget https://github.com/SagerNet/sing-box/releases/download/v1.3.0/sing-box_1.3.0_linux_amd64.deb
sudo dpkg -i sing-box_1.3.0_linux_amd64.deb
```

اگه با روش بالا نصب رو انجام بدین مسیر فایل کانفیگ میشه 
```
/et/sing-box/config.json
```

اینجا یه کانفیگ کلی معرفی میشه و به مرور فقط بخش های inbounds که مربوط به پروتکلهای مختلف هست رو میزارم .

برای نصب اتوماتیک روی نسخه های دیگه لینوکس هم اسکریپت کافگا رو پیشنهاد میدم.
```
bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/sing-box-yes/master/install.sh)
```


نمونه کانفیگی که گذاشتم رو اینجا به صورت بخش به بخش مشاهده میکنید ، توی نسخه جدید شما میتونید کل کانفیگ ها رو توی یه فایل ذخیره کنید و هم میتونید توی چند فایل فقط در حالت دوم باید برای اجرای برنامه از سوییچ C با حرف بزرگ نوشته میشه استفاده کنید .
شکل کلی دستور برای اجرا شبیه دستور زیره :
```
./sing-box -C /etc/sing-box
```

# DNS
```
    "dns": {
        "disable_cache": false,
        "disable_expire": false,
        "final": "dns-remote",
        "rules": [
            {
                "domain_suffix": [
                    "ir",
                    "firefox.com",
                    "ircf.space",
                    "speed.io",
                    "speedtest.net"
                ],
                "server": "dns-direct"
            },
            {
                "disable_cache": true,
                "domain_suffix": [
                    "app-measurement.com",
                    "crashlytics.com",
                    "google-analytics.com",
                    "appcenter.ms",
                    "firebase.io"
                ],
                "geosite": "category-ads-all",
                "server": "dns-block"
            },
            {
                "disable_cache": true,
                "domain_suffix": [
                    ".arpa.",
                    ".arpa"
                ],
                "server": "dns-block"
            }
        ],
        "servers": [
            {
                "address": "https://1.1.1.1/dns-query",
                "address_resolver": "dns-direct",
                "address_strategy": "prefer_ipv4",
                "strategy": "ipv4_only",
                "tag": "dns-remote"
            },
            {
                "address": "local",
                "detour": "Direct",
                "tag": "dns-direct"
            },
            {
                "address": "rcode://success",
                "tag": "dns-block"
            }
        ],
        "strategy": "ipv4_only"
    }
```
# Panel
```
    "experimental": {
        "clash_api": {
            "cache_file": "/etc/sing-box/cache.db",
            "default_mode": "rule",
            "external_controller": "0.0.0.0:9090",
            "external_ui": "/etc/sing-box/dashboard",
            "secret": "Panel_Password",
            "store_selected": true
        }
    }
```
# Inbounds
```
    "inbounds": [
        {
            "domain_strategy": "prefer_ipv4",
            "listen": "127.0.0.1",
            "listen_port": 2080,
            "sniff": true,
            "tag": "mixed-in",
            "type": "mixed"
        }
    ]
```
# log
```
    "log": {
        "level": "error"
    }
```
#Outbounds
```
    "outbounds": [
        {
            "tag": "Direct",
            "type": "direct"
        },
        {
            "tag": "Bypass",
            "type": "direct"
        },
        {
            "tag": "Block",
            "type": "block"
        },
        {
            "tag": "DNS-Out",
            "type": "dns"
        }
    ]
```
# Route
```
    "route": {
        "auto_detect_interface": true,
        "final": "Selector",
        "geoip": {
            "path": "geoip.db"
        },
        "geosite": {
            "path": "geosite.db"
        },
        "rules": [
            {
                "outbound": "DNS-Out",
                "port": 53
            },
            {
                "inbound": "dns-in",
                "outbound": "DNS-Out"
            },
            {
                "geoip": "private",
                "outbound": "Block"
            },
            {
                "geosite": "category-ads-all",
                "outbound": "Block"
            },
            {
                "domain_suffix": [
                    "appcenter.ms",
                    "app-measurement.com",
                    "firebase.io",
                    "crashlytics.com",
                    "google-analytics.com"
                ],
                "outbound": "Block"
            },
            {
                "geoip": "ir",
                "outbound": "Block"
            },
            {
                "domain_suffix": "ir",
                "outbound": "Block"
            },
            {
                "ip_cidr": [
                    "224.0.0.0/3",
                    "ff00::/8"
                ],
                "outbound": "Block",
                "source_ip_cidr": [
                    "224.0.0.0/3",
                    "ff00::/8"
                ]
            }
        ]
    }
```
