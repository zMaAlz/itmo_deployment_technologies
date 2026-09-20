# Cборка контейнера apache 2.4 с добавлением бибилиотек 1С сервера
=========

## Требуется для запуска

- docker.io
- docker-compose-v2
- (Для Windows) wsl v2 (Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux) 

### Состав образа

Базовый образ - httpd:2.4. 

В базовый образ добавляются библиотеки платформы 1С (пакеты ws, common, server)
Перед сборкой необходимо поместить в каталог \deb_1с deb пакеты 1С:
- 1c-enterprise-8.3.хх.хххх-common_8.x.xx-xxxx_amd64.deb
- 1c-enterprise-8.3.ххх.хххх-server_8.x.xx-xxxx_amd64.deb
- 1c-enterprise-8.3.хх.хххх-ws_8.x.xx-xxxx_amd64.deb


## Сборка контейнера

```bash
# Переходим в каталог с Dockerfile
$ cd ./docker_image/

# Запускаем сборку контенера 
$ docker build -t web-server-1c:8.3.27.2074 .

# Выводим список образов в локальном хранилище 
$ docker image ls

IMAGE                       ID             DISK USAGE   CONTENT SIZE   EXTRA
web-server-1c:8.3.27.2074   6fee0530dba9       3.61GB         1.45GB

# Для загрузки образа в реестр контейнеров
$ docker login 

$ docker push web-server-1c:8.3.27.2074

# Для выгрузки из реестра контейнеров (на целевом сервере)
docker pull web-server-1c:8.3.27.2074

```
### Прочее

В случае наличия проблем с доступом к wsap24.so в Dockerfile добавить "RUN chmod 645 /opt/1cv8/x86_64/8.3.xx.xxxx/wsap24.so"