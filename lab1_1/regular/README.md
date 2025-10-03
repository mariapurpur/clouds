# Лабораторная работа №1 (без звёздочки)

## Рәхим итегез!

![minecraft](https://github.com/user-attachments/assets/d085b42e-e520-4e65-84f6-911540d197a6)

## Подготовка

  Немного вчитавшись в условие заданий и то, где было бы удобнее сделать лабораторную работу, мы решили, что будем делать её на компьютере нашей коллеги — Галины Мошкиной, — так как она была единственной из нас с Линуксом как основной ОС.
  
  Первым шагом в выполнении лабораторной работы стало создание папок, в которых находятся страницы сайтов. После недолго обсуждения мы решили назвать их просто и лаконично: `Super-Roblox.com` и `Giper-Minecraft.com`.

<img width="1280" height="486" alt="image" src="https://github.com/user-attachments/assets/d2fc62bf-a1db-4356-94a3-9d60d093fbaa" />

<img width="1280" height="486" alt="image" src="https://github.com/user-attachments/assets/1d7138b2-30b8-4ba2-8202-e9a951a70412" />

  Вместо index.html мы прописали следующие команды, чтобы заменить обычное название файла:

`sudo nano minecraft-test.html`

`sudo nano roblox-test.html`

  После этого поменяли владельца на пользователя nginx и дали права на чтение и выполнение.

## Конфигурация

  Далее мы настроили конфигурацию сайта, чтобы мы могли перейти на сайт не только по домену `giper-minecraft.com`, но и `www.giper-minecraft.com`. Указываем, куда нам нужно переходить по директории, а если прохода нет, тогда выдаётся ошибка 404.

Для giper-minecraft.com:
```
server {
        listen 80;
        server_name giper-minecraft.com www.giper-minecraft.com;

        location / {
            root /var/www/giper-minecraft.com/html;
            index minecraft-test.html;
            try_files $uri $uri/ /minecraft-test.html =404;
        }
}
```

Для super-roblox.com:
```
server {
        listen 80;
        server_name super-roblox.com www.super-roblox.com;

        location / {
            root /var/www/super-roblox.com/html;
            index roblox-test.html;
            try_files $uri $uri/ /roblox-test.html =404;
        }
}
```

  Всё это мы делали в первый день и составляли схему brainstorm'а. Фото прилагается :)

  <img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/4a60b040-64fa-4d88-b102-e74d271f9e14" />

## Получение SSL-сертификата

  После первого дня мы осознали, что двигаемся намного медленнее, чем хотелось бы, и для ускорения нам нужно было немного поменять подход к задачам. Мы подумали, и составили новый план, который помог бы выполнить задания быстрее.

  Создаём папку и генерируем там SSL-сертификат, который отвечает за безопасное соезинение с сайтом.

  <img width="749" height="131" alt="image" src="https://github.com/user-attachments/assets/ab1b3d57-8146-4776-8f74-ff8c287056e7" />

  <img width="748" height="181" alt="image" src="https://github.com/user-attachments/assets/a13d6e14-39bf-405f-8b4f-b6ef1b86dc0e" />

  Ура, ключ сгенерирован!
  
  Для правильной работы сайта нам нужно сделать так, чтобы пользователь мог спокойно пользоваться сайтом. Для этого нам будет очень полезен Nginx. Он влияет на работу сайта, который после nginx будет загружаться быстрее, а так же балансирует поток пользователей на сайте.
  
  Внутренности файла `nginx.conf` выглядят следующим образом:

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
    worker_connections 768;
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    include /etc/nginx/sites-enabled/*;
}
```

  Немного изменив файлы `giper-minecraft.com` и `super-roblox.com`, они стали выглядеть следующим образом:

### giper-minecraft.com

<img width="746" height="679" alt="image" src="https://github.com/user-attachments/assets/9b4a440d-0427-4bc4-b3ce-f36755a80905" />

### super-roblox.com

<img width="744" height="680" alt="image" src="https://github.com/user-attachments/assets/84f48de4-4b88-4825-a729-fa78a2064a18" />

  Выводят они то же самое, что и раньше, просто теперь работают корректно.
  
  В аналогах этих файлов, а то есть на что перенаправляют giper-minecraft.com и super-roblox.com, лежат сами html файлы с сайтами. Их мы уже указывали в первой части лабораторной работы.

## Проблемы наступают

  После запуска возникла первая проблема: ничего не запускалось. После недолгих исселодваний оказалось, что причиной стал apache2, который мешался. Мы перезагрузили ПК, но ошибка осталась.
  
  <img width="752" height="423" alt="image" src="https://github.com/user-attachments/assets/766ab018-ddce-4949-b2b1-edb515376a93" />

  Проверяем, что слушают порты 80 и 433:
  
```
gala@gala-NMH-WDX9:~$ sudo ss -tulpn | grep :80
tcp   LISTEN 0      511                *:80               *:*    users:(("apache2",pid=1436,fd=4),("apache2",pid=1435,fd=4),("apache2",pid=1433,fd=4))
gala@gala-NMH-WDX9:~$ sudo ss -tulpn | grep :443
udp   UNCONN 0      0               [::]:44395         [::]:*    users:(("avahi-daemon",pid=947,fd=15)) 
```

  Решение, к счастью, оказалось простым: нужно было отключить apache2.

```
gala@gala-NMH-WDX9:~$ sudo systemctl stop apache2
gala@gala-NMH-WDX9:~$ sudo systemctl disable apache2
```

  Также необходимо добавить домены в файл hosts для локального тестирования через команду `sudo nano /etc/hosts`. Там дописываем следующее:

```
127.0.0.1 giper-minecraft.com www.giper-minecraft.com
127.0.0.1 super-roblox.com www.super-roblox.com
```

  Ура, проблема решилась!

## Результат

  Итак, наши сайты имеют SSL-сертификат и располагаются на одном сервере. Вот как это выглядит:

  <img width="748" height="193" alt="image" src="https://github.com/user-attachments/assets/06cd835d-fa51-4714-b3d4-6dbe63db519c" />

  <img width="748" height="451" alt="image" src="https://github.com/user-attachments/assets/640e4f0b-b644-4f43-a385-1e645e8eb38e" />

  <img width="745" height="420" alt="image" src="https://github.com/user-attachments/assets/d08eb7bd-84e3-4900-8dcb-c06fc3008995" />

  Проверили правильность написания конфигов командой `sudo nginx -t`, и перезапускаем nginx с помощью `sudo systemctl reload nginx`, чтобы всё заработало!
  
## Вывод

  Мы навайбили ssl-сертификат и настроили сайты под требования лабораторной :)
  
  После нескольких дней совместных разборов и многократных сессий самообучения, мы преодолели все трудности и сделали это. Наши лица теперь выглядят примерно так.
  
  <img width="816" height="763" alt="image" src="https://github.com/user-attachments/assets/ea5eb15b-d737-4e63-80ce-e24582b2b27f" />
