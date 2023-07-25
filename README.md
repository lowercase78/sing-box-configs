# Sing-Box Configs

قصد دارم اینجا کانفیگ‌های مربوط به سینگ باکس رو بزارم رفرنس اصلی سینگ باکس سایت خودشون هست ..
```
https://sing-box.sagernet.org
```




برای اجرای سینگ باکس نیاز هست که یه فایل کانفیگ با فرمت json براش تعریف بشه توی که مشخص میکنه که چه پروتها و پروتکلهایی رو اجرا کنه .

کلیات کانفیگ به شکل زیره :
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

اینجا یه کانفیگ کلی معرفی میشه و به مروز فقط بخش های inbounds که مربوط به پروتکلهای مختلف هست رو میزارم .

برای نصب اتوماتیک روی نسخه های دیگه لینوکس هم اسکریپت کافگا رو پیشنهاد میدم.
```
bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/sing-box-yes/master/install.sh)
```
