# Отчет по лабораторной работе №2  
## Создание кросс-провайдерной модели AWS и Azure
### Вариант 7

---

## Цель работы

Целью лабораторной работы являлось формирование единой модели классификации облачных сервисов, независимой от конкретного вендора.

В качестве базовой модели использовалась сервисная структура AWS, разработанная в лабораторной работе №1. Ее мы применили для классификации сервисов Microsoft Azure.

---

## Выполнение работы

Чтобы выполнить поставленную задачу, нам необходимо было проанализировать сервисы Azure на основе `Meter Category`, `Meter Sub-Category` и `Meter Name`, наийти аналог в модели AWS и сопоставить значения с уже существующими праметрами классификации.

---

## Классификация сервисов Microsoft Azure

### 1. Cloud Services  
*(Управляемые платформенные сервисы)*

#### Аналитика (Analytics)

**Azure SQL Database / Azure Synapse Analytics** — управляемые реляционные базы данных и аналитические хранилища.

- Использование: vCore, DTUs, Database Units, Storage  

Российский аналог: Yandex Managed Service for PostgreSQL, Postgres Pro (VK Cloud)

---

**Azure Data Factory v2** — сервис интеграции и оркестрации данных (ETL).

- Использование: Activity Runs, Data Movement, Pipeline Execution  

Российский аналог: Yandex Data Proc + Airflow / Yandex DataLens

---

#### Сервисы приложений (Application Services)

**Azure App Service (Websites)** — платформа для размещения и масштабирования веб-приложений.

- Использование: Hours, Free Tier, IP SSL  

Российский аналог: VK Cloud App Engine, Yandex Cloud Functions + API Gateway

---

**Visual Studio Team Services / Azure Application Insights** — инструменты DevOps и мониторинга приложений.

- Использование: Users, Agents, Insights  

Российский аналог: Yandex Tracker, Yandex Monitoring

---

### 2. Infrastructure  
*(Инфраструктура как услуга, IaaS)*

#### Вычисления (Compute)

**Azure Virtual Machines** — виртуальные серверы с различными конфигурациями и лицензиями ОС.

- Использование: Compute Hours  

Российский аналог: Yandex Compute Cloud, VK Cloud Virtual Machines

---

### 3. Storage  
*(Хранилища данных)*

**Azure Blob Storage / Azure Managed Disks** — объектное и блочное хранилище данных.

- Использование: Storage GB, Data Stored  

Российский аналог: Yandex Object Storage, Yandex Compute Cloud Disks

---

**Azure Backup / Azure Recovery Services Vault** — сервисы резервного копирования.

- Использование: Storage  

Российский аналог: Yandex Cloud Backup

---

## Выводы

В ходе лабораторной работы мы смогли успешно применить модель на основе AWS для структурирования сервисов Microsoft Azure и создать единую кросс-провайдерную модель классификации облачных сервисов.

