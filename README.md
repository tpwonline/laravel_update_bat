# laravel_update_bat

laravel服务器端自动更新脚本 
本内容为sh文件

```
#!/bin/bash
#git拉取并覆盖当前代码
cd /xxxx
git fetch --all && git reset --hard origin/master && git pull

#nginx重启
service nginx restart
cd xxx

#laravel生产模式下，刷新配置和路由
php artisan config:cache
php artisan route:cache

#laravel更新数据库变更
php artisan migrate

#重启队列（利用screen管理队列）
screen -ls|awk 'NR>=1&&NR<=100{print $1}'|awk '{print "screen -S "$1" -X quit"}'|sh
screen -d -m php artisan queue:work --tries=3
```
 
