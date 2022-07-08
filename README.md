Все дальнейшие действия были проверены при использовании
```
marozov@iMac-marozov ~ % vagrant -v
Vagrant 2.2.19
```
```
marozov@iMac-marozov vboxmanage -v
6.1.32r149290
```
CentOS Linux release 7.9 из Vagrant Cloud
```
[root@selinux ~]$ cat /etc/redhat-release
CentOS Linux release 7.9.2009 (Core)
```
Установил утилиты для работы с selinux
```
yum install policycoreutils-python setroubleshoot
```
Задание 1

В конфиге nginx поменял порт на 8099

выполняем
```
systemctl start nginx
```

```
Job for nginx.service failed because the control process exited with error code. See "systemctl status nginx.service" 
and "journalctl -xe" for details.
```
выполняем
```
sealert -a /var/log/audit/audit.log
```
Итак,

вариант 1

выполнил команду 
```
setsebool -P httpd_run_preupgrade 1
```
проверил запуск сервиса nginx, он заработал

варинат 2
```
semanage port -l |grep 80
```
```
http_port_t tcp 80, 81, 443, 488, 8008, 8009, 8443, 9000
```
```
semanage port -m -t http_port_t -p tcp 8099
```
```
http_port_t  tcp 8099, 80, 81, 443, 488, 8008, 8009, 8443, 9000

systemctl start nginx

systemctl status nginx.service

 nginx.service - The nginx HTTP and reverse proxy server
 
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   
   Active: active (running) since Mon 2022-07-06 18:03:01 UTC; 35s ago
   
  Process: 2601 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  
  Process: 2598 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  
  Process: 2597 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
  
 Main PID: 2603 (nginx)
 
   CGroup: /system.slice/nginx.service
   
           ├─2603 nginx: master process /usr/sbin/nginx
           
           └─2604 nginx: worker process
```


вариант 3

создаем модуль
```
ausearch -c 'nginx' --raw | audit2allow -M my-nginx 
semodule -i my-nginx.pp
```
запускаем и проверяем работу nginx

Задание 2

выбрал вариант с
```
setsebool named_write_master_zones on
```
он позваолит named менять зоны. Не только приведенную в стенде, но и другие , это понадобится для днс сервера. можно 
было ограничиться правами на зону поменяв контекст named.ddns.lab.view1.jnl

chapter2.log
