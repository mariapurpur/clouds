# Лабораторная работа №3 (со звёздочкой)

## Рәхим итегез...
  
<img width="400" height="420" alt="image" src="https://github.com/user-attachments/assets/f6de851b-7387-45af-bbd8-d1ccdfeabfdb" />  
  
## Введение  
  
> Да ща разберёмся за часика 2 и будет всё... *(с) Галина*  
  
  Именно так началась одна из наших самых трудных и времязатратных попыток выполнить лабораторную работу №3*. Как и было сказано, ушло на всю работу более полутора месяцев, и, хоть всё закончилось поражением, мы не могли молчать об эмоциях, которые испытали.  
  
  Мы очень надеемся, что наши ошибки помогут в будущем другим и научат, что терпение и труд всё перетрут. Но давайте не будем затягивать и приступим к нашим попыткам решения...  
  
## Этап первый: отрицание
  
  Это был самый обычный день. Мы посмотрели на ТЗ, и увидели, что нужно выполнить всего-то 4 пункта. *"Легко,"* — подумали мы. — *"Посидим, разберёмся, и всё будет в шоколаде"*. В этот же день мы доделали лабораторную работу №3, и нам казалось, что нет ничего невозможного.  
  
  Заходим на сайт AppSec Portal и следуем инструкции, которая гласит, что нужно сделать следующие шаги:  
  
  1. **Клонировать репозиторий.**
```
  git clone https://gitlab.com/whitespots-public/appsec-portal.git appsec-portal
```
  2. **Перейти в root директорию.**
```
  cd appsec-portal
```
  3. **Установить variables окружения.**
```
  ./set_vars.sh
```
  4. **Запустить AppSec Portal.**
```
  sh run.sh
```
  5. **Создать аккаунт суперпользователя.**
```
  docker compose exec back python3 manage.py createsuperuser --username admin
```
  
  Однако, мы не смогли зайти дальше третьего шага. Мы установили дефолтные параметры в соответствии с инструкцией на сайте, но выходила ошибка:  
  
  <img width="1280" height="275" alt="image" src="https://github.com/user-attachments/assets/fef0d079-19e9-4cbd-be20-54a1a7a32593" />  
  
  В недоумении, мы попробовали переустановить AppSec Portal и повторить все те же самые шаги. Ничего не изменилось. В попытке понять, что происходит, пишем в консоль `sh run.sh` и сразу после просим логи.  
```
gala@gala-NMH-WDX9:~/appsec-portal$ sh run.sh
WARN[0000] The "CORS_ORIGIN_WHITELIST" variable is not set. Defaulting to a blank string. 
WARN[0000] The "CORS_ORIGIN_WHITELIST" variable is not set. Defaulting to a blank string. 
WARN[0000] /home/gala/appsec-portal/docker-compose.yml: the attribute version is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 13/13
 ✔️ Network appsec-portal_default                   Created                 0.1s 
 ✔️ Container appsec-portal-rabbitmq-1              Healthy                12.9s 
 ✔️ Container appsec-portal-postgres-1              Healthy                11.0s 
 ✔️ Container appsec-portal-importer-1              Started                11.2s 
 ✔️ Container appsec-portal-jira_helper-1           Started                12.3s 
 ✘ Container appsec-portal-back-1                  Error                  13.8s 
 ✔️ Container appsec-portal-db_helper_importer-1    Created                 0.1s 
 ✔️ Container appsec-portal-db_helper_automation-1  Created                 0.1s 
 ✔️ Container appsec-portal-db_helper-1             Created                 0.1s 
 ✔️ Container appsec-portal-back_celery_worker-1    Created                 0.1s 
 ✔️ Container appsec-portal-nginx-1                 Started                12.8s 
 ✔️ Container appsec-portal-back_celery_beat-1      Created                 0.1s 
 ✔️ Container appsec-portal-importer_worker-1       Created                 0.1s 
dependency failed to start: container appsec-portal-back-1 is unhealthy
gala@gala-NMH-WDX9:~/appsec-portal$ docker logs appsec-portal-back-1
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
sh: can't open 'entrypoint_django.sh': No such file or directory
```
  Но что за `entrypoint_django.sh`? Мы попробовали создать пустой файл с таким же названием, чтобы понять, что идёт не так, но выходила новая ошибка: `dependency failed to start: container appsec-portal-back-1 is unhealthy`.  
  
  Как оказалось, у нас были проблемы с `run sh`, который мы запускали дважды. Файл .env выглядел так:  
```
GNU nano 7.2                                                               .env                                                                        
DOMAIN=http://localhost
IMAGE_VERSION=latest
JWT_PRIVATE_KEY=-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQC2rozmQmireGrIwXJexgdUv43DxLgAXPFZ9BuFQYQ8q9yoDa9m4j47j3E>
JWT_PUBLIC_KEY=-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtq6M5kJoq3hqyMFyXsYHVL+Nw8S4AFzxWfQbhUGEPKvcqA2vZuI+O49xCGq8DRH7u>
SECRET_KEY=t0hR0RVmZoB4HufqJDQxddcMZ1gKjz37AifcXM8K
```
  Решаем снова всё снести и перезагрузить. Может, хотя бы так заработает?  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/4d6ca38b-7c37-41b5-946d-169921ec0131" />  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/77f6efcc-1480-4e18-b23a-d95396f523ae" />  
  
  Снова неудача, но в этот раз из-за nginx. Останавливаем его и пробуем снова.  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/3bafbe5a-dbff-4568-9adb-59fd44cf8021" />  
  
  Ничего, а всё потому что мы забыли рестартнуть контейнеры.  
  
  Тут мы уже начали ощущать, что это будет долго и мучительно...  

## Этап второй: перезапуск, перезапуск, перезапуск...  
  
  Пока мы снова сносили установку и составляли её заново, пришли к выводу, что DevOps невозможен без этих четырёх всадников апокалипсиса:  
  1. `docker compose ps`
  2. `docker compose down`
  3. `docker compose up -d`
  4. `docker logs`
  
  Но не забываем о ходе работы. Приступаем к очередной переустановке.  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/bc7d8424-b72f-4788-ad1f-ddc4e08a20d4" />  
  
  Теперь он не находит `entrypoint_django.sh`. Радует, что ошибка на этот раз другая, но не радует то, сколько ещё переустановок нам, скорее всего, придётся сделать, чтобы это заработало. Не отчаиваемся, просим помощи, устанавливаем другую версию.  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/9b273f27-3532-49fa-b861-159558dc8223" />  
  
  Не работает. Сносим.  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/1d95c9a5-c8bd-4a2e-877f-cee44ba3931f" />  
  
  Снова не работает. Снова сновим.  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/1e557143-b357-43b9-a9c1-ab1841240cef" />  
  
  Может, дело в VPN? Сносим...  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/0683065c-895c-4d76-affa-db6c5ca08415" />  
  
  Не помогло. Чтобы удостовериться в неправильности, мы попробовали переустановить всё ещё несколько раз с VPN в предыдущих версиях. Это тоже не помогло. Тогда мы зафиксировали версию `release_v25.07.1` и обратились к вам, чтобы получить совет, как всё исправить, и следовали строго по инструкции.  
  
  <img width="615" height="96" alt="image" src="https://github.com/user-attachments/assets/ec49be53-b5df-41ce-b6c9-7e8bd5225b9c" />  
  
  Подбадривание сработало, но ровно до следующей ошибки. Попробовали запустить с VPN, ничего не получилось.  

  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/8edf5f29-2a18-4d42-8095-4d5c549df1d6" />  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/cf133b0d-64ed-49d7-a127-fdfdc4225804" />  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/ee418ef3-2228-4fa4-b792-d1ada238c572" />  

  Снова провал. Мы были на грани отключения ноутбука и сомнения в чистоте нашего сознания. Всё, о чём мы могли думать — запуск, переустановка, полный снос, и так по кругу, пока не вылезет очередной сбой.  
  
  <img width="1084" height="753" alt="image" src="https://github.com/user-attachments/assets/4bf5c9c5-5224-4541-9f4f-cd2c0d07f8b2" />  
  
  Произошёл троллинг. Из последних сил, мы решили снова снести и переустановить всё, что есть.  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/64197b4f-50fd-4bbd-90ec-dbcaf723ef1f" />  
  
  О чудо...  
  
  <img width="1280" height="685" alt="image" src="https://github.com/user-attachments/assets/13d8cb5d-a7d4-4bd2-86b1-3a00e31693ab" />  
  
### Оно работает!  

  **Но как нам это удалось?** Оказывается, что проблема крылась в release notes разработчиков в их тг-канале (что крайне некомпетентно и вообще нам не понятно, почему это крылось в ТЕЛЕГРАММЕ, а не на основной странице). После пробы этой версии, всё запустилось без проблем.  

  Нашей радости не было предела. Мы смеялись, когда возвращались домой, ведь знали, что теперь осталось не очень много. Мы наконец-то сможем сдать лабораторную работу и порадоваться свободной жизни!  

  > *Как же мы ошибались...*  

## Этап третий: буря лицензии  
  
  <img width="621" height="435" alt="image" src="https://github.com/user-attachments/assets/cfcb0b9f-93c6-4565-aae4-318fcbd85237" />  
  
  Спустя 5 дней мы вновь садимся за лабораторную работу, и замечаем, что всё провалилось. Ничего снова не работает, выдавая нам те же самые ошибки. Мы не сломлены, но искренне разочарованы.  

  Почему-то в наших логах выходит giper-minecraft.com ещё из первой лабораторной работы, что стало идентификатором неправильной очистки прошлой переустановки. Снова сделаем снос и попробуем восстановиться. И действительно, теперь всё прошло гладко, за исключением одного **но**...  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/289b7707-5924-45a8-8f5c-d3787cc7d470" />  
  
  `License has expired`? Но как это возможно?  
  
  Мы обратились к вам, чтобы найти причину, и пришли к выводу, что стоит попробовать с другого пк нашей коллеги-сокомандницы: Ани. Мы переустанавливали версии такими же способами, как до этого, постарались сделать всё, но так же встречались с ошибкой. Наши предположения упали на то, что у ключа действительно закончился срок годности, хоть в это и не сильно верилось.  
  
  <img width="1086" height="225" alt="image" src="https://github.com/user-attachments/assets/391c5919-3151-4941-986d-3b2877574a38" />  
  
  Нас начинали бояться, но этого ли мы хотели? Да. Да, хотели. А ещё больше мы хотели добить эту лабораторную работу, чтобы всё было идеально и работало хотя бы частично.  

  <img width="783" height="414" alt="image" src="https://github.com/user-attachments/assets/1b17a1bc-dced-48fd-a7cc-2264ab7e4d6c" />  
  <img width="1214" height="768" alt="image" src="https://github.com/user-attachments/assets/230fe3bc-c7ed-4487-8f14-8e08674b1fa2" />  
  <img width="1214" height="768" alt="image" src="https://github.com/user-attachments/assets/2890caa6-521d-4ae7-b9b4-3218b2116d1e" />  
  
  После недолгих проверок, у Ани всё заработало, но во время вписания ключа возникала та же самая ошибка: `License has expired`. Верна ли наша гипотеза?  
  
  <img width="522" height="355" alt="image" src="https://github.com/user-attachments/assets/845a69b3-e88b-44fb-91b4-07b76fa81b53" />  
  
  На удивление, да, ключи были действительно просрочены. Our honest reaction to that information:  

  <img width="225" height="225" alt="image" src="https://github.com/user-attachments/assets/9676e26d-7141-495c-be85-37eb2f716020" />  

  Казалось бы, ключ есть, всё есть, надо только войти и выполнить работу.  

## Этап четвёртый: крик о помощи  
  
  <img width="631" height="109" alt="image" src="https://github.com/user-attachments/assets/3f8f8042-1569-4c77-b57e-96ec02202e74" />  

  Мы потеряли веру в то, что это какое-то совпадение, и нам просто везёт, поэтому уже относились к выполнению этой лабораторной как к чему-то рофельному. Очередное падение привело нас в смех (будь то истерический или обычный), но так же оставляло недоумевание.  

  Пробуем переустановить на двух версиях: 25.11.1 и 25.12.1.  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/14f6b20a-5d03-490d-8e88-40fd52a4131f" />  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/e1c43fc4-b687-448e-9719-2f73e6c2c50b" />  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/a8c2814e-5cbc-48b6-979e-7a6fbb3b88e8" />  
  
  Оказывается, недоснесли...  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/498f1208-2e6c-4548-a8c7-9a840a65ec93" />  
  
  Вобщем, у нас в этот раз получилось и мы решили вставить свой кружок:  
  
  https://github.com/user-attachments/assets/cc83d0a4-65ec-4a1c-9a17-fad7586d9731
  
  И вот, сработает ли всё наконец-то? Получится ли у нас закончить эту лабораторную работу? Ответ, конечно прост: *нет*.  
  
  <img width="1080" height="500" alt="image" src="https://github.com/user-attachments/assets/456278ce-9d32-4119-b2fe-a6e2ae2fc450" />  
  
  Он содеиняется, но не работает, что странно, ведь должен.  
  
  <img width="1280" height="678" alt="image" src="https://github.com/user-attachments/assets/2f4c63c6-d3b9-4d13-b4a5-0435f90bac4e" />  
  <img width="1280" height="678" alt="image" src="https://github.com/user-attachments/assets/df2baa29-3ddd-4676-a99e-ae644666b230" />  
  
  Пробуем по новой, потому что всё подключилось изначально неправильно, и хочется всё бросить и переустановить. Так и делаем и предварительно просим ключи.  
  
  <img width="1074" height="175" alt="image" src="https://github.com/user-attachments/assets/5498a1db-3e13-474f-8225-7af8d3442d1c" />  
  <img width="531" height="292" alt="image" src="https://github.com/user-attachments/assets/b873717d-e68d-4bc1-b7ff-afc296e3e9de" />  
  
> **Небольшая ремарка:** мы действительно придём на факультатив, это гарантировано!!  
  
  <img width="1094" height="692" alt="image" src="https://github.com/user-attachments/assets/96504543-d64d-4fc3-bd10-fe1e543f7cbd" />  
  
  В порыве очередного отчаяния, мы начали писать одногруппникам, которые всё успешно сделали. Мы не будем называть их имён, но спасибо каждому из них за то, что постарались нам помочь. А ещё мы сами стали консультантами по облачным вопросам.  
  
  <img width="644" height="150" alt="image" src="https://github.com/user-attachments/assets/fd8cca96-b668-4dec-84c2-372965fd43cd" />  
  
  Как оказалось, проблема была далеко не очевидной и даже удивительной. Всё это время нам нужно было сначала установить Auditor, и уже только потом AppSec. Мы делали ровно наоборот, потому что так было указано в оригинальном ТЗ.  
  
  <img width="788" height="131" alt="image" src="https://github.com/user-attachments/assets/a39df955-9473-4da8-a31f-764cfd869629" />  
  <img width="1280" height="682" alt="image" src="https://github.com/user-attachments/assets/b6e70d8e-8f3c-45a7-a19c-ec06daa65ee0" />  

  После длительных мук и сотен переустановок, мы, кажется, на финишной прямой. Нам оставалось проверить только наличие уязвимостей. Нас явно обрадовало, что пайплайны проходят, но это ещё ничего не значит. Они должны быть ненулевыми, но выходят на 0.  
  
## Этап пятый: Felina  

  Мы попробовали несколько разных вариантов, чем проверять на уязвимости:  
  
  1. Vulnerable PHP App.
  2. Vulnerable Java Android.
  3. Vulnerable Apps (вообще все).
  4. Лабораторная работа №2 по ООП.
  
  ...Но ничего не сработало.  
  
  <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/5aaa7512-f64d-4d70-a57c-c6838ea2baa6" />
  
  Нам сказали ещё поспрашивать у ребят, но, если честно, сил уже не было, и у тех, у кого мы спрашивали, всё получалось довольно легко. Складывалось ощущение, что единственные, кто не могут решить ошибки — мы. Как сказала наша коллега-соучастница Галина: *"Люди, которые не столкнулись с нашими ошибками — божественные существа, кто это вообще, как они в ИТМО учатся..."*  

  Последняя предпринятая попытка над этой лабораторной была оформлена Галей. Она сидела с ней 23 декабря на протяжении 15 часов. Когда она в 4 утра увидела очередное `findings 0`, что-то ёкнуло глубоко в душе, и желание пытаться дальше закончилось.  
  
  <img width="599" height="155" alt="image" src="https://github.com/user-attachments/assets/39379f2f-4372-4ddb-b3be-aeb7c518b2e3" />  
  
  Мы действительно были одни из первых, кто сел за эту работу, и так и не смогли выполнить задуманное. Мы сломлены, но всё это точно было не зря. Нас сплотил DevOps и заставил ботать столько, сколько нам и не снилось. Хоть у нас и остаётся неприятный осадок печали, было крайне приятно поработать над этим проектом.  
  
  Спасибо за поддержку и возможность. На вас держится DevOps. А мы наконец-то поспим...  
  
  <img width="733" height="705" alt="image" src="https://github.com/user-attachments/assets/71e1aa36-9f7e-4ab8-acb3-7d6a454cac28" />  
