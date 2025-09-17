# ansible-infrastructure

Automation kit for showcasing DevOps skills with Ansible. Репозиторий включает роли и плейбуки для Docker, PostgreSQL, Nginx, приложений, резервного копирования и мониторинга.

## Структура
- `playbooks/` — сценарии развёртывания по сервисам и средам
- `roles/` — переиспользуемые роли (docker, postgres, nginx, app, backups, monitoring, proxy)
- `group_vars/` и `inventories/` — переменные и хостовые группы для dev/stage/prod

## Плейбуки
- `playbooks/site.yml` — полный стек: Docker, Nginx, PostgreSQL, Monitoring
- `playbooks/docker.yml` — установка Docker Engine и Compose
- `playbooks/postgres.yml` — конфигурация PostgreSQL кластера
- `playbooks/nginx.yml` — настройка реверс-прокси Nginx
- `playbooks/app.yml` — пример деплоя сервисного Python-приложения
- `playbooks/backups.yml` — расписание systemd таймера и сервиса для бэкапов
- `playbooks/proxy.yml` — отдельное развертывание L7-прокси
- `playbooks/monitoring.yml` — подготовка Docker-окружения, деплой Prometheus + Alertmanager + Grafana, проверка здоровья Grafana API

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
- Используются community-коллекции (например `community.docker`) для управления docker-compose
- Шаблоны Jinja2 для конфигурации сервисов и provisioning Grafana
- Переменные окружений и секреты отделены через Ansible Vault и `group_vars`
- Постпроверки в плейбуках (например health-check Grafana) помогают демонстрировать практику верификации деплоя

Дополняйте репозиторий новыми сценариями (например hardening или CI runner), чтобы расширить портфолио и показать владение сумежными стеком инструментов.
