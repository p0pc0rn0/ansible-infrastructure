# ansible-infrastructure

Automation kit for showcasing DevOps skills with Ansible. Репозиторий включает роли и плейбуки для Docker, PostgreSQL, Nginx, приложений, резервного копирования и мониторинга.

## Структура
- `playbooks/` — сценарии развёртывания по сервисам и средам
- `roles/` — переиспользуемые роли (docker, postgres, nginx, app, backups, monitoring, proxy)
- `group_vars/` и `inventories/` — переменные и хостовые группы для dev/stage/prod

## Плейбуки
- `playbooks/site.yml` — единая оркестрация, последовательно импортирует сервисные плейбуки (docker → proxy → postgres → app → monitoring → backups)
- `playbooks/docker.yml` — установка Docker Engine/Compose и проверка демона
- `playbooks/postgres.yml` — конфигурация PostgreSQL с автотюнингом sysctl и проверкой подключения
- `playbooks/nginx.yml` — настройка реверс-прокси и HTTP health-check
- `playbooks/app.yml` — деплой Python-приложения в контейнере, прогон docker role, SDK и проверка ответа через Nginx
- `playbooks/backups.yml` — запуск роли бэкапов на `db_hosts` с выбором env (`-e env=prod|stage`), итоговая сводка статуса задач
- `playbooks/proxy.yml` — параметризуемая настройка системных прокси с валидацией окружения
- `playbooks/monitoring.yml` — подготовка Docker-окружения, деплой Prometheus + Alertmanager + Grafана и проверки API/targets

## Быстрый старт
```bash
# Проверить инвентори
tree inventories/

# Провалидировать плейбук мониторинга в режиме check
ansible-playbook -i inventories/dev/hosts.ini playbooks/monitoring.yml --check

# Развернуть прод инфраструктуру целиком
ansible-playbook -i inventories/prod/hosts.ini playbooks/site.yml
```

## Особенности
- Используются community-коллекции (например `community.docker`) для управления docker-compose и инспекции контейнеров
- Шаблоны Jinja2 для конфигурации сервисов, Alertmanager и provisioning Grafana
- Переменные окружений и секреты отделены через Ansible Vault и `group_vars`; инвентори сгруппированы (`webservers`, `dbservers`) для многослойного деплоя
- Постпроверки (Docker info, Prometheus targets, HTTP health-check) показывают практику верификации деплоя

Дополняйте репозиторий новыми сценариями (например hardening или CI runner), чтобы расширить портфолио и показать владение смежным стеком инструментов.
