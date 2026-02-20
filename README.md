🚀 LLM Infrastructure as Code (Ansible + Docker)

Production-style IaC проект для автоматического развертывания:

🤖 Ollama (LLM сервер)

📊 Prometheus

📈 Grafana

🖥 Node Exporter

🐳 Docker Engine

Полностью воспроизводимый стек через Ansible.

🏗 Архитектура

Control Node:

Debian / Linux машина

Ansible установлен

Управляет серверами по SSH

Target Node:

Ubuntu Server 24.04

Чистая система (достаточно SSH и Python3)

Настраивается автоматически

📂 Структура проекта
llm-iac/
├── group_vars/
│   └── llm_nodes.yml        # Переменные для группы llm_nodes
├── inventory/
│   └── hosts.ini            # Список управляемых серверов
├── roles/
│   ├── common/              # Базовая настройка системы
│   ├── docker/              # Установка Docker
│   ├── llm/                 # Развертывание Ollama
│   └── monitoring/          # Prometheus + Grafana + Node Exporter
├── site.yml                 # Главный playbook
└── README.md
⚙️ Что делает каждая роль
common

apt update

apt upgrade

установка базовых пакетов

docker

установка Docker Engine

запуск сервиса

добавление пользователя в docker group

llm

копирование docker-compose.yml

запуск Ollama

публикация API на порт 11434

monitoring

Prometheus (9090)

Node Exporter

Grafana (3000)

Автоматическое provisioning:

Datasource Prometheus

Дашборд Node Exporter

📋 Требования

На control node:

sudo apt install ansible git

SSH доступ к target node:

ssh user@server_ip

Python3 должен быть установлен на target (обычно уже есть).

🖥 Настройка inventory

Файл:

inventory/hosts.ini

Пример:

[llm_nodes]
server1 ansible_host=192.168.56.101 ansible_user=alex

Можно добавить несколько серверов:

[llm_nodes]
server1 ansible_host=192.168.56.101 ansible_user=alex
server2 ansible_host=192.168.56.105 ansible_user=alex
🔧 Переменные

Файл:

group_vars/llm_nodes.yml

Пример:

llm_stack_dir: /opt/stack/llm
monitoring_stack_dir: /opt/stack/monitoring

Можно расширять:

версии образов

порты

пароли

🚀 Развертывание
Полная установка
ansible-playbook -i inventory/hosts.ini site.yml --ask-become-pass
Запуск только определенной роли

Только Docker:

ansible-playbook -i inventory/hosts.ini site.yml --ask-become-pass --tags docker

Только LLM:

ansible-playbook -i inventory/hosts.ini site.yml --ask-become-pass --tags llm

Только мониторинг:

ansible-playbook -i inventory/hosts.ini site.yml --ask-become-pass --tags monitoring
🔍 Проверка после установки
Ollama API
http://server_ip:11434

Проверка:

curl http://server_ip:11434/api/tags
Prometheus
http://server_ip:9090
Grafana
http://server_ip:3000

Default credentials (если не изменены):

admin / admin

Дашборд:

Folder: Infra

Dashboard: Node Exporter Full
