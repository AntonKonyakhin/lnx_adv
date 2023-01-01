2. Установите пакет nginx-full.

```
apt install nginx-full
```

3. Напишите конфигурационный файл nginx, который реализует следующую функциональность:
- Отключен модуль mail
```
rm /etc/nginx/modules-enabled/50-mod-mail.conf
```
- В отдельном конфигурационном файле должен быть описан server, который слушает на 80 порту и использует домен, полученный при помощи сервиса http://nip.io/. Например, Ваш публичный адрес 100.101.102.103, тогда один из вариантов имени будет app.100.101.102.103.nip.io. Если этот сервис недоступен, то можно воспользоваться любым аналогичным.

```
создать файл в папке /etc/nginx/sites-available
```
конфиг
```
server {
  listen 80;
  server_name 127.0.0.1;

  index index.nginx-debian.html;

  access_log /var/log/nginx/nip.access.log logz;


  location / {
    root /var/www/html/;
  }

  location /rbm_images {
    alias /var/www/rebrain/images/;

  }
  location =/rbm_images/logo.png {
    alias /var/www/rebrain/images/logo_rebrain_black.png;
  }



}                                                                                                 
```

- Используется формат логов с именем logz, который содержит только информацию о том, откуда был произведен запрос, в какое время, какой был произведен запрос и какой HTTP код был возвращен при запросе.

```
в секции http определить log_format
```

```
log_format logz '$remote_addr - [$time_local] - "$request" - $status';
```

- Логи должны писаться в файл /var/log/nginx/nip.access.log.
```
access_log /var/log/nginx/nip.access.log logz;
```

- В роли root используется /var/www/html/.
- В роли index - /var/www/html/index.nginx-debian.html, которая должна возвращаться при обращении к /.
- Путь /rbm_images/* должен отдавать файлы из /var/www/rebrain/images/, где должен быть расположен файл http://rebrainme.com/files/logo_rebrain_black.png под именем logo.png.

4. В ответе пришлите:
- конфигурационный файл для основного сервера и путь до него;

```
/etc/nginx/sites-available
```

- список файлов в /etc/nginx/modules-enabled/.
```
root@ubuntu-lvm:/etc/nginx/sites-available# ls -la /etc/nginx/modules-enabled/
total 24
drwxr-xr-x 2 root root 4096 янв  1 20:14 .
drwxr-xr-x 8 root root 4096 янв  1 22:30 ..
lrwxrwxrwx 1 root root   57 янв  1 19:48 50-mod-http-auth-pam.conf -> /usr/share/nginx/modules-available/mod-http-auth-pam.conf
lrwxrwxrwx 1 root root   56 янв  1 19:48 50-mod-http-dav-ext.conf -> /usr/share/nginx/modules-available/mod-http-dav-ext.conf
lrwxrwxrwx 1 root root   53 янв  1 19:48 50-mod-http-echo.conf -> /usr/share/nginx/modules-available/mod-http-echo.conf
lrwxrwxrwx 1 root root   55 янв  1 19:48 50-mod-http-geoip2.conf -> /usr/share/nginx/modules-available/mod-http-geoip2.conf
lrwxrwxrwx 1 root root   54 янв  1 19:48 50-mod-http-geoip.conf -> /usr/share/nginx/modules-available/mod-http-geoip.conf
lrwxrwxrwx 1 root root   61 янв  1 19:48 50-mod-http-image-filter.conf -> /usr/share/nginx/modules-available/mod-http-image-filter.conf
lrwxrwxrwx 1 root root   60 янв  1 19:48 50-mod-http-subs-filter.conf -> /usr/share/nginx/modules-available/mod-http-subs-filter.conf
lrwxrwxrwx 1 root root   62 янв  1 19:48 50-mod-http-upstream-fair.conf -> /usr/share/nginx/modules-available/mod-http-upstream-fair.conf
lrwxrwxrwx 1 root root   60 янв  1 19:48 50-mod-http-xslt-filter.conf -> /usr/share/nginx/modules-available/mod-http-xslt-filter.conf
lrwxrwxrwx 1 root root   50 янв  1 19:48 50-mod-stream.conf -> /usr/share/nginx/modules-available/mod-stream.conf

```
