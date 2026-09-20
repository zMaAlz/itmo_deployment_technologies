# Ansible playbook для развертывания web-сервера c публикацией 1C

## Требуется для запуска
 
- Ansible 2.10 и новее
- Коллеция community.docker (ansible-galaxy collection install community.docker)
- (на Windows) wsl v2 (Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux) 

## Переменные

- 1C_HOSTNAME - имя веб-сервера (для Nginx)
- LB_HOSTNAME_PORT - порт балансировщиа (для Nginx)
- WORKDIR - каталог на сервере для размещения файлов web-сервера 1C
- IMG_1C_WEB_SERVER - Ссылка на образ в реестре контейнеров
- REGISTRY_URL - реестр контейнеров
- REGISTRY_USER - учетная запись для доступа к реестру контейнеров 
- REGISTRY_PASS - пароль или токен для доступа к реестру контейнеров 

## Структура проекта

- group_vars\all.yaml - Переменные 
- inventory\hosts.yaml - Адрес сервера для установки
- deploy_1C-web-server.yaml - шаги развертывания web-сервера 1C. 
- templates/docker-compose-yaml.j2 - шаблон docker-compose манифеста для запуска контейнера 1С 
- templates/1C-web-server/ - конфигурационные файлы монтируемые в контейнер

* httpd.conf - файл конфигурации apache 
* inetpub/ - каталог с файлами публикации 1С (default.vrd)
* nginx/ - каталог с конфигурацией nginx
* secret/ - каталог с сертификатами для nginx

### Прочее

В качетсве цели требуется севрер с ОС - Astra Linux 1.8 

В процессе развертывания на сервере будут установлены пакеты: docker.io, docker-compose-v2, python3-pip + модуль requests

https://github.com/Live-AG/EasyVRD - обработка позволяет сформировать default.vrd

## Команды запуска playbook

```bash
# запуск в режиме тестирования (без внесения изменений)
ansible-playbook -i inventory/hosts.yaml deploy_web_server_1с.yaml --diff -v  --ask-become-pass --check

# запуск в обычном режиме
ansible-playbook -i inventory/hosts.yaml deploy_web_server_1с.yaml --diff -v --ask-become-pass
```
