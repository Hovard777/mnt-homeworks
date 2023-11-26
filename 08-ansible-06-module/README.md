# Домашнее задание к занятию 6 «Создание собственных модулей»

## Основная часть

**Шаг 4.** Проверьте module на исполняемость локально.
```bash
(venv) ifebres@ifebres-nb:~/netology/devops/ansible$ python -m ansible.modules.my_own_module /tmp/param.json

{"changed": true, "ok": false, "failed": false, "path": "", "message": "Created", "invocation": {"module_args": {"path": "/home/ifebres/file.txt", "content": "Hello, this is the content of the file."}}}
```

**Шаг 5.** Напишите single task playbook и используйте module в нём.
```yaml
---
- name: Test module
  hosts: localhost
  tasks:
    - name: Call my_test_module
      my_own_module:
        path: /tmp/file.txt
        content: Hello world
```

**Шаг 6.** Проверьте через playbook на идемпотентность.
```yaml
(venv) ifebres@ifebres-nb:~/netology/devops/ansible$ ansible-playbook test.yml 
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible engine, or trying out features under development. This is a rapidly changing source of
code and can become unstable at any point.
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Test module] *****************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************************************************************
ok: [localhost]

TASK [Call my_test_module] *********************************************************************************************************************************************************************************************************
changed: [localhost]

PLAY RECAP *************************************************************************************************************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```


**Шаг 8.** Инициализируйте новую collection: `ansible-galaxy collection init my_own_namespace.yandex_cloud_elk`.
```yaml
ifebres@ifebres-nb:~/netology/devops/ansible$ ansible-galaxy collection init my_own_namespace.yandex_cloud_elk
[WARNING]: You are running the development version of Ansible. You should only run Ansible from "devel" if you are modifying the Ansible engine, or trying out features under development. This is a rapidly changing source of
code and can become unstable at any point.
- Collection my_own_namespace.yandex_cloud_elk was created successfully

```

**Шаг 11.** Создайте playbook для использования этой role.
```yaml
---
- name: Test module
  hosts: localhost
  collections:
    - my_own_namespace.yandex_cloud_elk

  roles:
    - my_own_role
```

**Шаг 15.** Установите collection из локального архива: `ansible-galaxy collection install <archivename>.tar.gz`.
```yaml
ifebres@ifebres-nb:~/tmp$ ansible-galaxy collection install my_own_namespace-yandex_cloud_elk-1.0.0.tar.gz 
Starting galaxy collection install process
Process install dependency map
Starting collection install process
Installing 'my_own_namespace.yandex_cloud_elk:1.0.0' to '/home/ifebres/.ansible/collections/ansible_collections/my_own_namespace/yandex_cloud_elk'
my_own_namespace.yandex_cloud_elk:1.0.0 was installed successfully
```

**Шаг 16.** Запустите playbook, убедитесь, что он работает.
``` yaml
ifebres@ifebres-nb:~/tmp$ ansible-playbook test1.yml 
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Test module] ************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************
ok: [localhost]

TASK [my_own_namespace.yandex_cloud_elk.my_own_role : Call my_test_module] ****************************************************************************************************************************************
ok: [localhost]

PLAY RECAP ********************************************************************************************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

**Шаг 17.** ссылки на collection и tar.gz архив, а также скриншоты выполнения пунктов 4, 6, 15 и 16.

***Collection*** [Collection](https://github.com/Hovard777/my_own_collection/tree/1.0.0)  
***Archive*** [my_own_namespace-yandex_cloud_elk-1.0.0.tar.gz](my_own_namespace-yandex_cloud_elk-1.0.0.tar.gz)