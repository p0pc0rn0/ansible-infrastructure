## Backup Playbook (Production-ready)

## Описание

Централизованная система бэкапов с поддержкой PostgreSQL, MySQL и файловых директорий.
Метрики записываются в формат Prometheus, таймер запускается на Control Node через systemd.

## Требования

	•	Ansible >= 2.9
	•	Node Exporter на db_hosts
	•	SSH доступ с control node к db_hosts
	•	systemd на control node

## Установка

1.	Клонируем репозиторий и переходим в директорию:
```
git clone https://github.com/p0pc0rn0/ansible-infrastructure.git
cd ansible-infrastructure
```
 2.	Установить переменные для prod (секреты через Vault):
```
ansible-vault edit group_vars/prod.yml
```
3.	Проверить соединение с хостами:
```
ansible -i inventories/prod/hosts.ini db_servers -m ping
```

⸻

## Первый запуск

1.	Выполнить плейбук вручную для проверки:
```
ansible-playbook playbooks/backup.yml -i inventories/prod/hosts.ini -e env=prod
```
2.	Убедиться, что бэкапы созданы на db_hosts и файлы rsync перенесены на удалённый сервер.
3.	Активировать systemd timer на control node:
```
sudo systemctl daemon-reload
sudo systemctl enable --now ansible-backup.timer
sudo systemctl status ansible-backup.timer
```
Таймер запускает бэкапы каждый день в 03:00 (по расписанию в backup.timer.j2).

4.	Проверка метрик:
```
	•	Node Exporter на db_hosts: /var/lib/node_exporter/textfile_collector/backup.prom
	•	Prometheus собирает метрики автоматически
	•	Настройка алертов через Prometheus rules (Slack/Telegram/Email интеграции возможны через group_vars)
```
