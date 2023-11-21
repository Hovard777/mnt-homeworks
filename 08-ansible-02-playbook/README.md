
# Описание решения домашнего задания 2 «Работа с Playbook»

Playbook находится в файле [site.yml](playbook/site.yml).

Inventory находится в файле [prod.yml](playbook/inventory/prod.yml).

Переменные находятся в файле [vars.yml](playbook/group_vars/clickhouse/vars.yml).

Playbook устанавливает сервисы **clickhouse** и **vector**.

Для установки сервиса **clickhouse** с сайта разработчика устанавливаются пакеты:
- clickhouse-common-static
- clickhouse-server
- clickhouse-client

После этого создаётся БД **logs**.


Для установки сервиса **vector** с сайта разработчика устанавливаются пакеты:
- vector

После, из шаблона создаётся конфигурационный файл `{{vector_config_dir}}/vector.toml` и настраивается сервис.

Playbook использует следующие переменные:

- **clickhouse_version** - требуемая версия **clickhouse**
- **vector_config_dir** - путь до конфигурационного файла **vector**


