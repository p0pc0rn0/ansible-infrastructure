# 🚀 ansible-infrastructure

Ansible-инфраструктура для DevOps/DevSecOps. Репозиторий собирает production-friendly плейбуки и роли для установки Docker-стека, публикации приложений, мониторинга, резервного копирования и обеспечения безопасности.

## 📂 Структура репозитория
```
ansible-infrastructure/
|- ansible.cfg
|- inventories/
|  |- dev/hosts.ini
|  |- stage/hosts.ini
|  |- prod/hosts.ini
|- group_vars/
|  |- all.yml
|  |- dev.yml
|  |- prod.yml
|- playbooks/
|  |- site.yml
|  |- docker.yml
|  |- proxy.yml
|  |- postgres.yml
|  |- app.yml
|  |- monitoring.yml
|  |- backups.yml
|  |- baseline-hardening.yml
|  |- security-audit.yml
|  |- cicd-jenkins.yml
|  |- ansible-runner.yml
|  |- terraform-provision.yml
|  |- vault.yml
|  |- elk.yml
|  |- zabbix-server.yml
|  |- zabbix-agent.yml
|- roles/
|  |- app/
|  |- backups/
|  |- baseline_hardening/
|  |- docker/
|  |- elk_stack/
|  |- jenkins_ci/
|  |- monitoring/
|  |- nginx/
|  |- postgres/
|  |- proxy/
|  |- security_audit/
|  |- semaphore/
|  |- terraform_provision/
|  |- vault/
|  |- zabbix_agent/
|  |- zabbix_server/
```

## 🔑 Основные возможности
- Docker-first оркестрация — роль `docker` ставит Engine/Compose (включая обход для Ubuntu Noble) и готовит рантайм для остальных стеков.
- Сервисные плейбуки — готовые сценарии `proxy`, `postgres`, `nginx`, `app` и `vault` для настройки сетевых прокси, БД, ревёрс-прокси и секретов.
- Наблюдаемость и резервирование — стек Prometheus+Grafana+Alertmanager, текстовые метрики резервных копий и демо ELK-кластер.
- CI/CD и автоматизация — Jenkins с Groovy bootstrap и Ansible Semaphore UI (`ansible-runner.yml`) для self-service запусков.
- Безопасность и соответствие — baseline hardening, OpenSCAP/Lynis аудит, systemd таймеры бэкапов и Vault dev secrets.
- Интеграции и демо-инфраструктура — Terraform -> Ansible конвейер, плейбуки Zabbix server/agent и сценарии гибридного развёртывания.

## 🗂️ Ключевые плейбуки
| Playbook | Цель | Что важно |
| --- | --- | --- |
| `playbooks/site.yml` | Единая оркестрация | Импортирует docker -> proxy -> postgres -> app -> monitoring -> cicd-jenkins -> backups |
| `playbooks/baseline-hardening.yml` | Базовая защита | Админ-группа, SSH hardening, auditd, unattended-upgrades, fail2ban, UFW |
| `playbooks/security-audit.yml` | Соответствие и отчётность | OpenSCAP и Lynis, отчёты в `security_audit_output_dir`, сводка по каждому хосту |
| `playbooks/cicd-jenkins.yml` | CI/CD демо | Jenkins master, Groovy bootstrap пользователя/пайплайна, готов к сборке Docker-образов |
| `playbooks/monitoring.yml` | Наблюдаемость | Docker Compose для Prometheus, Alertmanager, Grafana provisioning и health-check цель |
| `playbooks/terraform-provision.yml` | Provision + config | Вызывает роль `terraform_provision`, добавляет хосты и запускает Ansible пост-конфигурацию |

## 🚀 Быстрый старт
1. Клонируем репозиторий:
   ```bash
   git clone https://github.com/p0pc0rn0/ansible-infrastructure.git
   cd ansible-infrastructure
   ```
2. Настраиваем переменные окружения (пример для prod, защищено Vault):
   ```bash
   ansible-vault edit group_vars/prod.yml
   ```
3. Проверяем подключение к хостам выбранного окружения:
   ```bash
   ansible -i inventories/prod/hosts.ini all -m ping
   ```
4. Запускаем сводный плейбук для окружения:
   ```bash
   ansible-playbook playbooks/site.yml -i inventories/prod/hosts.ini -e env=prod
   ```

## 🧪 Практические сценарии
- `ansible-playbook -i inventories/dev/hosts.ini playbooks/baseline-hardening.yml --check` — оценка харднинга в dry-run режиме.
- `ansible-playbook -i inventories/dev/hosts.ini playbooks/security-audit.yml` — сбор OpenSCAP и Lynis отчётов в `/var/reports/security`.
- `ansible-playbook -i inventories/dev/hosts.ini playbooks/cicd-jenkins.yml -e jenkins_pipeline_repo_url=https://github.com/your/repo` — поднять Jenkins и подключить пайплайн.
- `ansible-playbook playbooks/terraform-provision.yml` — пример конвейера Terraform -> Ansible с импортом динамических хостов.

## 🛡 Production-friendly подход
- Отдельные `inventories` и `group_vars` для dev/stage/prod, секреты через Ansible Vault.
- Роли идемпотентны и переиспользуемы (Docker runtime, сервисы, безопасность, мониторинг).
- Пост-проверки сервисов: Docker info, HTTP health-check, Prometheus targets, вывод Vault/ELK URL.
- Systemd таймеры и текстовые метрики для контроля резервных копий и аудитов.
- Поддержка гибридных сценариев: Terraform, Zabbix, локальный Vault и ELK под Docker.

## 📌 Планы на будущее
- Kubernetes cluster bootstrap — Ansible + kubeadm для control plane и worker узлов.
- CI/CD demo: Flask App -> Docker -> Deploy via Ansible — сквозной пайплайн от кода до прод-сервера.
- Load Balancer (HAProxy или Keepalived + Nginx) — отказоустойчивый входной трафик для web tier.
- Security & Compliance — расширение с OPA/Conftest и отчётностью в централизованный storage.
- Cloud / Hybrid — модули для AWS/Azure и сочетание Terraform + Ansible inventory plugins.
- Data / Message Queue — роли для Kafka/RabbitMQ и интеграция с существующим мониторингом.

## 👤 Автор
👨‍💻 Руслан (DevOps/DevSecOps Engineer)
📫 Контакт: [@p0pc0rn0](https://github.com/p0pc0rn0)
