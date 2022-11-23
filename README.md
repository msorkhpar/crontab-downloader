# crontab-downloader
یکی از روش هایی که احتمال میره باعث این بشه که سرور های وی پی ان رو شناسایی کنن، برابر بودن مقدار ترافیک ورودی و خروجی سروری که روش وی پی ان راه انداخته شدست. اسکیریپتی که اینجا میبینید برای سرور های واسط ایران استفاده میشه تا فایلی رو توی بازه های زمانی تصادفی دانلود کنه تا ترافیک ورودی بیشتر از ترافیک خروجی باشه.

# نصب wget

``` shel
#Ubuntu
sudo apt install -y wget

#Centos 7
sudo yum install -y wget

#Fedora & Centos 8
sudo dnf install -y wget
```

# ایجاد فایل اسکریپت
قبل از اجرای دستور زیر مقادیر یو ار ال رو به ادرس فایل هایی که توی ایران هستن تغییر بدید تا به صورت تصادفی یکی از این فایل ها توسط سرور دانلود شود
```shell
tee $HOME/donwloader.bash << DOWNLOADER
#!/bin/bash

urls=("http://1" "http://2" "http://3")
len=${#urls[@]}
index=$(($RANDOM%$len))

delay=$(($RANDOM%400))
sleep $delay

/usr/bin/wget -T 5 -t 1 ${urls[$index]} -O /dev/null -o /dev/null
DOWNLOADER

chmod +x $HOME/donwloader.bash

# حالا باید فایل را به سیتسم عامل برای اجرای زمان دار معرفی کنیم
crontab -e

# بعد از اجرای دستور قبل، خط زیر را به انتهای فایل باز شده اضافه کنید
*/15 * * * * bash $HOME/donwloader.bash 2>&1 >> /dev/null
