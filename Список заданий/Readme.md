# Docker — Самостоятельные работы

Создание Docker-контейнеров из готовых образов.

> Никогда не используйте русские имена файлов и каталогов!  
> Никогда не используйте пробелы и спец.символы в именах файлов и каталогов!

---

## Содержание

1. [Welcome to Docker](#1-welcome-to-docker)
2. [Adminer](#2-adminer)
3. [MySQL](#3-mysql)
4. [MongoDB](#4-mongodb)
5. [Portainer](#5-portainer)
6. [Speedtest](#6-speedtest)
7. [PostgreSQL](#7-postgresql)
8. [Ubuntu](#8-ubuntu)
9. [Pcb2gcode](#9-pcb2gcode)
10. [Alt Linux](#10-alt-linux-в-docker)

---

## 1. Welcome to Docker

Официальный ознакомительный образ Docker.

Проверить, что порт 8088 свободен (Windows):
```shell
netstat -aon | findstr :8088
```
![фе](/Список%20заданий/img/1.1.png)

Проверить порт (Linux/Mac/WSL):
```shell
netstat -tuln | grep :8088
```

Запустить контейнер:
```shell
docker run -d -p 8088:80 --name welcome-to-docker docker/welcome-to-docker
```
![фе](/Список%20заданий/img/1.2.png)
Открыть в браузере: http://localhost:8088
![фе](/Список%20заданий/img/1.3.png)
Войти внутрь контейнера:
```shell
docker exec -it welcome-to-docker /bin/sh
```
![фе](/Список%20заданий/img/1.4.png)
Команды внутри контейнера:
```shell
uname -a
top
apk update && apk upgrade
apk add fastfetch
fastfetch
```
![фе](/Список%20заданий/img/1.5.png)
Остановить и удалить:
```shell
docker stop welcome-to-docker
docker rm welcome-to-docker
```
![фе](/Список%20заданий/img/1.6.png)
---

## 2. Adminer

Adminer — веб-интерфейс для управления БД.

> Перед созданием убедитесь, что порт 8084 не занят другим приложением!

Запустить контейнер:
```shell
docker run -d --name adminer -p 8084:8080 adminer:latest
```

Открыть в браузере: http://localhost:8084
![фе]()
> Без отдельно запущенного контейнера с БД панель работать не будет!  
> Для подключения к PostgreSQL:
> - Система: PostgreSQL  
> - Сервер: host.docker.internal  
> - Логин: postgres  
> - Пароль: mysecretpassword

Управление:
```shell
docker stop adminer
docker start adminer
docker rm adminer
```
![фе]()
---

## 3. MySQL

MySQL — реляционная СУБД.

Запустить контейнер:
```shell
docker run -d \
  --name my-mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=mydb \
  -e MYSQL_USER=user \
  -e MYSQL_PASSWORD=password \
  mysql:8
```
![фе](/Список%20заданий/img/2.1.png)
Подключиться к MySQL:
```shell
docker exec -it my-mysql mysql -u root -p
```

> Пароль: rootpassword
![фе](/Список%20заданий/img/2.2.png)
Команды SQL для проверки:
```sql
SHOW DATABASES;
USE mydb;
CREATE TABLE test (id INT PRIMARY KEY, name VARCHAR(50));
INSERT INTO test VALUES (1, 'Docker');
SELECT * FROM test;
EXIT;
```
![фе](/Список%20заданий/img/2.3.png)
Управление:
```shell
docker start my-mysql
docker stop my-mysql
docker rm my-mysql
```
![фе](/Список%20заданий/img/2.4.png)
---

## 4. MongoDB

MongoDB — документоориентированная NoSQL СУБД.

Запустить контейнер:
```shell
docker run -d \
  --name my-mongo \
  -p 27017:27017 \
  mongo:latest
```
![фе](/Список%20заданий/img/3.1.png)
Подключиться через mongosh:
```shell
docker exec -it my-mongo mongosh
```
![фе](/Список%20заданий/img/3.2.png)
Команды в mongosh:
```javascript
show dbs
use mydb
db.users.insertOne({ name: "Alex", age: 21 })
db.users.find()
exit
```
![фе](/Список%20заданий/img/3.3.png)
Управление:
```shell
docker stop my-mongo
docker start my-mongo
docker rm my-mongo
```

---

## 5. Portainer

Portainer — графический веб-интерфейс для управления Docker.

Запустить контейнер:
```shell
export MSYS_NO_PATHCONV=1
```

```shell
docker run -d \
  --name portainer \
  -p 9000:9000 \
  -p 9443:9443 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  --restart unless-stopped \
  portainer/portainer-ce:latest
```
![фе](/Список%20заданий/img/4.1.png)

Открыть в браузере: http://localhost:9000
![фе](/Список%20заданий/img/4.2.png)

Создать пароль администратора (минимум 8 символов) и войти в панель.

Основные возможности:
- Управление контейнерами: создание, запуск, остановка, удаление
- Просмотр логов в реальном времени
- Мониторинг CPU и RAM
- Pull образов из Docker Hub
- Управление сетями и томами
- Развёртывание Docker Compose (Stacks)

![фе](/Список%20заданий/img/4.3.png)

Управление:
```shell
docker stop portainer
docker rm portainer
```

---

## 6. Speedtest

Локальный сервер для теста скорости интернета.

> ⚠️ Может некорректно работать в РФ из-за блокировок РКН.

Запустить контейнер:
```shell
docker run -d -p 158:80 --name speedtest-server adolfintel/speedtest
```
![фе](/Список%20заданий/img/5.1.png)
Открыть в браузере: http://localhost:158/
![фе](/Список%20заданий/img/5.2.png)
Управление:
```shell
docker stop speedtest-server
docker rm speedtest-server
```
![фе]()
---

## 7. PostgreSQL

PostgreSQL — объектно-реляционная СУБД.

Запустить контейнер:
```shell
docker run -d \
  --name my-postgres \
  -p 5432:5432 \
  -e POSTGRES_PASSWORD=mysecretpassword \
  postgres:alpine
```
![фе](/Список%20заданий/img/6.1.png)
Подключиться через psql:
```shell
docker exec -it my-postgres psql -U postgres
```
Команды SQL для проверки:
```sql
\l
CREATE DATABASE testdb;
\c testdb
CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(50));
INSERT INTO users (name) VALUES ('Docker');
SELECT * FROM users;
\q
```
![фе](/Список%20заданий/img/6.2.png)
Управление:
```shell
docker stop my-postgres
docker start my-postgres
docker rm my-postgres
```

---

## 8. Ubuntu

Ubuntu — популярный Linux-дистрибутив для тестирования команд.

> Контейнер удаляется автоматически после выхода (`--rm`)

Запустить временный контейнер:
```shell
docker run -it --rm ubuntu:latest /bin/bash
```
![фе](/Список%20заданий/img/7.1.png)

> Если появится ошибка `TLS handshake timeout` — проигнорируйте и запустите команду повторно.

Установить что-нибудь внутри:
```shell
apt update && apt install -y neofetch curl
neofetch
curl --version
```
![фе](/Список%20заданий/img/7.2.png)
Выйти из контейнера:
```shell
exit
```

---

## 9. Pcb2gcode (ngargaud/insolante)

Веб-оболочка для Pcb2gcode — конвертация Gerber-файлов в G-code для ЧПУ.

### Linux / macOS / Windows

```shell
export MSYS_NO_PATHCONV=1
mkdir -p ~/insolante_data

MSYS_NO_PATHCONV=1 docker run --rm -p 8081:5000 -d \
  -e URL=http://localhost \
  -e RPORT=8180 \
  -e DEBUG=false \
  -v ~/insolante_data:/opt/core/data \
  ngargaud/insolante

```
![фе](/Список%20заданий/img/8.1.png)
Открыть в браузере: http://localhost:8081


Введите пароль (например `123`) и войдите в панель.
![фе](/Список%20заданий/img/8.2.png)
---

## 10. Alt Linux в Docker

Выполнить все этапы работы с образом Alt по примеру задания с Nginx.

### Загрузить готовый образ Alt

```shell
docker pull alt:sisyphus
```
Запустить и использовать
```shell
docker run -ti --rm --name alt alt:sisyphus /bin/bash
```
![фе](/Список%20заданий/img/9.1.png)

После запуска вы окажетесь внутри контейнера Alt Linux с правами root.

Установить приложение fastfetch в контейнере
```shell
apt-get update && apt-get install fastfetch
```
![фе](/Список%20заданий/img/9.2.png)

Запустить fastfetch
```shell
fastfetch
```
![фе](/Список%20заданий/img/9.3.png)

Убедитесь, что утилита выводит информацию о системе (дистрибутив, ядро, ресурсы).

Выйти из контейнера с Alt
```shell
exit
```
После выхода контейнер будет автоматически удалён (использован параметр --rm).