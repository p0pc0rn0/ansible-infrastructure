# Backup Playbook (Production-ready)

## Описание
Централизованная система бэкапов с поддержкой PostgreSQL, MySQL и файловых директорий.  
Метрики пишутся в формат Prometheus, таймер запускается на Control Node через systemd.  

## Требования
- Ansible >= 2.9
- Node Exporter на db_hosts
- SSH доступ с control node к db_hosts
- systemd на control node

## Установка
1. Клонируем репозиторий:
```bash
git clone https://github.com/p0pc0rn0/ansible-infrastructure.git
cd ansible-infrastructure