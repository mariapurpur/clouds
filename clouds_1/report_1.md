# Отчет по лабораторной работе №1  
## Анализ сервисной модели AWS
### Вариант 7

---

## Цель работы

Данная лабораторная работа была направлена на первичное структурирование облачных сервисов на примере Amazon Web Services (AWS). Основная задача заключалась в преобразовании сырых биллинговых данных, содержащих SQL-подобные шаблоны (например, `%DataTransfer%`), в четкую сервисную модель.


---

## Теория

Для начала мы изучили необходимую для выполнения задания теорию. Необходимо было разобраться в том, какие в целом есть модели облачных сервисов, чтобы создать их иерархию.

### IaaS (Infrastructure as a Service)

Предоставляет базовые вычислительные ресурсы: виртуальные машины, сети и диски.  
Пользователь управляет операционными системами и приложениями.


### PaaS (Platform as a Service)

Предоставляет готовую платформу для развертывания и эксплуатации приложений (базы данных, среды выполнения, аналитические сервисы).  
Пользователь управляет только кодом и данными.

Большинство сервисов в анализируемом биллинге относятся именно к этой категории.

### SaaS (Software as a Service)

Предоставляет полностью готовое программное обеспечение.  
Пользователь управляет только данными и настройками внутри приложения.

---

## Выполнение работы

Чтобы выполнить классификацию, мы сопоставляли строки биллинга (Product Code, Usage Type) с официальной документацией Amazon Web Services `https://docs.aws.amazon.com/`.


---

## Классификация сервисов AWS

### 1. Cloud Services  
*(Управляемые платформенные и прикладные сервисы)*

#### Безопасность и идентификация (Security and Identity)

**Amazon Macie** — сервис для автоматического обнаружения, классификации и защиты конфиденциальных данных.

- Компонент использования: Data Protection 

Российский аналог: Kaspersky Hybrid Cloud Security, Security Vision (VK Cloud) 


---

#### Аналитика (Analytics)

**Amazon MSK (Managed Streaming for Kafka)** — управляемый сервис потоковой обработки данных.

Компоненты потребления:
- Run Broker (Compute)
- Storage GP2 (Storage)
- Data Transfer (Networking)

Российский аналог: Yandex Data Streams


---

**Amazon OpenSearch Service** — управляемый сервис поиска и анализа логов и данных.

Компоненты потребления:
- Instance Hours (Compute)
- GP2 Storage (Storage)
- Data Transfer (Networking)

Российский аналог: Yandex Managed Service for OpenSearch

---

#### Сервисы приложений (Application Services)

**Amazon MSK (Run Broker)** — вычислительная составляющая платформы потоковой обработки данных.

- Использование: Run Broker  

Российский аналог: Yandex Managed Service for Apache Kafka

---

#### Искусственный интеллект (Artificial Intelligence)

**Amazon Polly** — сервис синтеза речи из текста.

- Использование: Text-to-Speech  

Российский аналог: Yandex SpeechKit

---

**Amazon Personalize** — сервис построения персонализированных рекомендаций.

Компоненты потребления:
- Real-Time Inference  
- Training Hour  
- Data Ingestion  

Российский аналог: Yandex Recommendations

---

### 2. Storage  
*(Хранилища данных)*

#### Storage & Content Delivery

**Amazon S3 (Simple Storage Service)** — масштабируемое объектное хранилище данных.

Категории использования:
- Storage (Byte Hours, Glacier, Intelligent-Tiering)
- Data Transfer
- Requests (GET, PUT)
- Management

Российский аналог: Yandex Object Storage, VK Cloud S3

---

### 3. Networking  
*(Сетевые сервисы)*

**Amazon CloudFront** — сеть доставки контента (CDN).

- Использование: Data Transfer Out  

Российский аналог: Yandex CDN, VK Cloud CDN


---

## Выводы

В ходе лабораторной работы мы выделили ключевые категории и уровни классификации сервисов, структурировали биллинговые данные AWS и получили сервисную модель.  

