# Лабораторная работа №3 (со звёздочкой)

## Рәхим итегез!
  

  
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
  
#### Оно работает!  

  **Но как нам это удалось?** Оказывается, что проблема крылась в release notes разработчиков в их тг-канале. После пробы этой версии, всё запустилось без проблем.   

  Нашей радости не было предела. Мы смеялись, когда возвращались домой, ведь знали, что теперь осталось не очень много. Мы наконец-то сможем сдать лабораторную работу и порадоваться свободной жизни!  

  > *Как же мы ошибались...*  

## Этап третий: буря  

## Этап четвёртый: крик помощи  

## Этап пятый: Felina  

---

# План (удалим)

  34. спустя 5 дней - Таня приве всё сломалось (скрин 19), в недоумении
  35. в логах почему-то всплывает giper-minecraft.com, оказалось, что недочистили комп
  36. СНОВА СНОС
  37. license has expired (скрин 20), ключ за ключом не работают
  38. дроп от Тани (сообщение 7)
  39. кучу раз сносили и виртуалку, и всё остальное включая версии latest и все перечисленные выше, но ничего не помогало, Таня боится (скрин 21)
  40. пару раз посносили на компе гали, license expired
  41. Анна Журавлёва пишет, что у неё всё работает и при этом не работает, А НА ПК ГАЛИНЫ МОШКИНОЙ НЕТ (скрин 22-23)
  42. СЮЖЕТНЫЙ ПОВОРОТ ключи были просрочены (ЧТОООООО??????) (скрин и сообщение 24)
  43. реакт гали на это (скрин 25)
  44. приве у нас опять всё упало (скрин 26-29)
  45. выснилось, что недоснесли (скрин 30)
  46. кружок
---
  48. мы НЕВЕРОЯТНО везучие, и хоть всё запустилось, ничего снова не работает (скрин 31)
  49. (скрин 34-35) коннектед, но не работает.... 
  50. нужен ещё ключ (скрин 32)
  51. (скрин 33 и сообщения)
  52. зов помощи с Артемием Поповым (попробовали взять воркшоп и подействовать по их тутору, но не помогло)
  53. из тутора поняли, что сначала Auditor, а потом Appsec, всё это время мы делали неправильно
  54. мы личные консультанты по облакам (скрин 36)
  55. КАК ВЫЯСНИЛОСЬ ошибка в том, что сначала скачивается аудитор, потом портал, а мы делали наоборот, потому что так было в ТЗ (скрин 37)
  56. (сообщения последние)
  57. по кд сносы и переустановки
  58. (скрин 38) pipelines проходят, но это ещё ничего не значит))))) выводило findings 0
---
  59. мы попробовали с двумя отдельными репами vulnerable apps (vulnerable php app, vulnerable java android, вообще все через аудитор, а ещё прогнали лабу 2 по C#), но всё равно findings 0. (скрин 39)
  60. сказали посмотреть у ребят, но там ничего не было из наших ошибок, чем крайне раздосадована Галя (верит, что это божественное существо, которое не столкнулось с ошибками, что такие люди не должны учиться в итмо и вообще кто это....)
  61. (скрин 40)
  62. короче всё не зря, но мы выебанные и очень расстроенные((( потому что последняя попытка была 15-ти часовая до 4 часов утра. когда всплыло findings 0 в 4 утра, подумала, что нахуй надо.
  63. конец.....

**КОНЕЦ**  

ЗАМАЗАТЬ МАТ  
<img width="736" height="709" alt="image" src="https://github.com/user-attachments/assets/9e303c30-62cb-41ac-8279-1763af57297d" />  
