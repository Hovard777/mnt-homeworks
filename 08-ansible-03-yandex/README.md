Playbook устанавливает Clichouse, Vector, LightHouse на виртуальные машины.

**clickhouse**  
с сайта разработчика устанавливаются пакеты:
- clickhouse-common-static
- clickhouse-server
- clickhouse-client

После этого создаётся БД **logs**.

**vector**   
с сайта разработчика устанавливаются пакеты:
- vector

После, из шаблона создаётся конфигурационный файл `{{vector_config_dir}}/vector.toml` и настраивается сервис.

**LightHouse**  
- устанавливается в /opt/lighthouse
- дополнительно устанавливается epel, nginx

Playbook находится в файле [site.yml](playbook/site.yml).

Inventory находится в файле [prod.yml](playbook/inventory/prod.yml).

Переменные:
- Clickhouse [vars.yml](playbook%2Fgroup_vars%2Fclickhouse%2Fvars.yml)
- Vector [vars.yml](playbook%2Fgroup_vars%2Fvector%2Fvars.yml)
- LightHouse [lighthouse.yml](playbook%2Fgroup_vars%2Flighthouse%2Flighthouse.yml)



