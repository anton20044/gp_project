| **<br/>Проектная работа по курсу Greenplum для разработчиков и архитекторов баз данных<br/>Проектирование production-ready хранилища данных для e-commerce на базе Greengage<br/>**|



Проект разворачивает 11 ВМ: 
  
  - 1 с ролью маршрутизатора
  - 1 с ролью СУБД PostgreSQL
  - 1 с ролью DNS
  - 1 с ролью СУБД Clickhouse
  - 1 с ролью хранилища s3 MiniO
  - 6 с ролью GreenGage (мастер, резервный мастер и 4 сегмент хоста)

<br/>
## * Перед разворачиванием в VirtualBox должен быть создан bringe интерфейс.
## * Перед разворачиванием необходимо сгенерировать сертификат
<br/>

### Генерация сертификата
* Установить OpenSSL
* Создать файл [san.cnf](san.cnf)
* Актуализировать в нем имена и IP адреса
* Выполнить команду:
```
openssl req -config san.cnf -new -x509 -sha256 -newkey rsa:2048 -nodes -keyout key.pem -days 365 -out key.cert.pem
```
* Будут сформированы файлы key.pem (содержит закрытый ключ) и key.cert.pem (содержит публичный ключ)
* Добавить файлы сертификатов в роли Ansible

### Запуск проекта
```
#Разворачивание ВМ с помощью Vagrant'а
vagrant up dns01
vagrant up router01
vagrant up pg01
vagrant up clickhouse01
vagrant up minio01
vagrant up gpmaster
vagrant up gpstandby
vagrant up gpseg01
vagrant up gpseg02
vagrant up gpseg03
vagrant up gpseg04

#Настройка ВМ с помощью Ansible
ansible-playbook -i ansible/hosts ansible/playbook.yml -l dns01
ansible-playbook -i ansible/hosts ansible/playbook.yml -l router01
ansible-playbook -i ansible/hosts ansible/playbook.yml -l pg01
ansible-playbook -i ansible/hosts ansible/playbook.yml -l minio01
ansible-playbook -i ansible/hosts ansible/playbook.yml -l clickhouse01
ansible-playbook -i ansible/hosts ansible/playbook.yml -l gpmaster,gpstandby,gpseg01,gpseg02,gpseg03,gpseg014
```

* После разворачивания необходимо произвести сборку и установку PXF (PXF пока не собирается ролью, происходит только подготовка окружения и скачивание исходников).
* Необходимо настроить утилиту s3cmd на сервере PostgreSQL.
* За основу взять тестовый датасет PostgresPro авиаперевозки.

** Файл [DV.txt](DV.txt)содержит описание хранилища
** Файл [DV_load_data.txt](DV_load_data.txt)содержит описание загрузки данных их PostgreSQL в GreenGage, через S3
** Файл [sql_import.txt](sql_import.txt)содержит описание скрипты для загрузки данных из PostgreSQL в S3
** Файл [query.txt](query.txt)содержит описание запросы и описание получения данных в Clickhouse
