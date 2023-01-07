

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

- Разрешены обращения со всех хостов, кроме 10.10.10.10

```
создать файл в папке /etc/nginx/sites-available
```

```
apt install apache2-utils

```

htpasswd -c /etc/nginx/htpasswd $USERNAME данная команда создаст (за это отвечает ключ -c)

Если же требуется добавить новую запись в уже существующий файл, то можно сделать это, вызвав htpasswd /etc/nginx/htpasswd $USERNAME_NEW.



конфиг
```
upstream backend {
  server example.com:443;
}
deny 10.10.10.10

#resolver 8.8.8.8 ipv6=off;

log_format logz '$remote_addr - [$time_local] - "$request" - $status';

server {
  listen 80;
  server_name app.nip.io;
  index index.nginx-debian.html;

  access_log /var/log/nginx/nip.access.log logz;

  auth_basic "Administrator's area";
  auth_basic_user_file /etc/nginx/.htpasswd;

  root /var/www/html/;

  location / {

  }

  location /noauth/ {
    auth_basic off;
  }

  location /rbm_images {
    alias /var/www/rebrain/images/;
  }

  location =/rbm_images/logo.png {
    alias /var/www/rebrain/images/logo_rebrain_black.png;
  }

  location /example/ {
    resolver 1.1.1.1 ipv6=off;
    proxy_pass https://backend;
    proxy_set_header Host example.com;
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


Запрос на /example/ должен проксироваться на https://example.com/, обращение к которому производится через upstream.

```
http {

       upstream backend {
         server example.com:443;
       }
}
```

```

location /example/ {
    resolver 1.1.1.1 ipv6=off;
    proxy_pass https://backend;
    proxy_set_header Host example.com;
  }

  
```



