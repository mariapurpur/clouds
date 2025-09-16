# Лабораторная работа №1 (без звёздочки)

## Рәхим итегез!

![minecraft](https://github.com/user-attachments/assets/d085b42e-e520-4e65-84f6-911540d197a6)

## Подготовка

  Первым шагом в выполнении лабораторной работы стало создание папок, в которых находятся страницы сайтов. После недолго обсуждения мы решили назвать их просто и лаконично: `Super-Roblox.com` и `Giper-Minecraft.com`.

<img width="1280" height="486" alt="image" src="https://github.com/user-attachments/assets/d2fc62bf-a1db-4356-94a3-9d60d093fbaa" />

<img width="1280" height="486" alt="image" src="https://github.com/user-attachments/assets/1d7138b2-30b8-4ba2-8202-e9a951a70412" />

  Вместо index.html мы прописали следующие команды, чтобы заменить обычное название файла:

`sudo nano minecraft-test.html`

`sudo nano roblox-test.html`

  После этого поменяли владельца на пользователя nginx и дали права на чтение и выполнение.

## Конфигурация

  Далее мы настроили конфигурация сайта, чтобы мы могли перейти на сайт не только по домену `giper-minecraft.com`, но и `www.giper-minecraft.com`. Указываем, куда нам нужно переходить по директории, а если прохода нет, тогда выдаётся ошибка 404 (кстати, она у нас кастомная).

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

## Написание nginx.conf

  Так как теперь у нас есть сайты, мы должны их перенести на сервер и начать работу с `nginx.conf`.

  Nginx отвечает за "коммуникацию" между клиентом и сервером. Для полноценной работы нам нужно написать файл `nginx.conf`, который находится в директории `/etc/nginx`.

  Мы создали `/sites-conf`, `/var`.

  <img width="983" height="691" alt="image" src="https://github.com/user-attachments/assets/734c24c5-1444-4f16-a7ac-7c4f9687d410" />

  <img width="983" height="691" alt="image" src="https://github.com/user-attachments/assets/07423fc1-7021-4533-aa51-e05fee9bbd99" />

  <img width="984" height="178" alt="image" src="https://github.com/user-attachments/assets/7476027f-9680-4db2-a741-507177e13e6e" />

  <img width="984" height="178" alt="image" src="https://github.com/user-attachments/assets/32aae63e-d617-44d6-a6a6-6e1ea32b2f2f" />

  Внутренности файла `nginx.conf` выглядят следующим образом:
  
## Вывод

  Мы навайбили ssl-сертификат и настроили сайты под требования лабораторной :)
