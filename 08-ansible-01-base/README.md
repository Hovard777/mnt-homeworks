# mnt-homeworks
# Домашнее задание к занятию "08.01 Введение в Ansible"

</details>  

## Подготовка к выполнению

### Установите ansible версии 2.10 или выше.

```bash
  $ ansible --version
ansible 2.10.8
  config file = None
  configured module search path = ['/home/ifebres/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jun 11 2023, 05:26:28) [GCC 11.4.0]

```

## Основная часть

1. Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте какое значение имеет факт `some_fact` для указанного хоста при выполнении playbook'a.
   ```bash
   $ ansible-playbook ./playbook/site.yml -i ./playbook/inventory/test.yml
   PLAY [Print os facts] ****************
   TASK [Gathering Facts] ***************
   ok: [localhost]
   TASK [Print OS] **********************
   ok: [localhost] => {
    "msg": "Ubuntu"
   }
   TASK [Print fact] ********************
   ok: [localhost] => {
    "msg": 12
   }
   PLAY RECAP ***************************
   localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   ```
2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.
   ```bash
   $ cat group_vars/all/examp.yml
   ---
     some_fact: "all default fact"
   ```
3. Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.
   ```bash
   $ sudo docker ps
   CONTAINER ID   IMAGE                      COMMAND       CREATED          STATUS          PORTS     NAMES
   b226b9715a4d   pycontribs/ubuntu:latest   "/bin/bash"   56 seconds ago   Up 55 seconds             ubuntu
   9760c04b667d   centos:7                   "/bin/bash"   56 seconds ago   Up 55 seconds             centos7
   ```
4. Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.
   ```bash
   $ ansible-playbook site.yml -i inventory/prod.yml
   PLAY [Print os facts] **************************************************************************************************************************************************************************************
   
   TASK [Gathering Facts] *************************************************************************************************************************************************************************************
   [DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release 
   will default to using the discovered platform python for this host. See https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be 
   removed in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
   ok: [ubuntu]
   ok: [centos7]
   
   TASK [Print OS] ********************************************************************************************************************************************************************************************
   ok: [centos7] => {
       "msg": "CentOS"
   }
   ok: [ubuntu] => {
       "msg": "Ubuntu"
   }
   
   TASK [Print fact] ******************************************************************************************************************************************************************************************
   ok: [centos7] => {
       "msg": "el"
   }
   ok: [ubuntu] => {
       "msg": "deb"
   }
   
   PLAY RECAP *************************************************************************************************************************************************************************************************
   centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   ```
5. Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились следующие значения: для `deb` - 'deb default fact', для `el` - 'el default fact'.
   ```bash
   $ cat group_vars/{deb,el}/*
   ---
     some_fact: "deb default fact"
   ---
     some_fact: "el default fact"
   ```
6. Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.
   ```bash
   $ ansible-playbook site.yml -i inventory/prod.yml
   PLAY [Print os facts] **************************************************************************************************************************************************************************************

   TASK [Gathering Facts] *************************************************************************************************************************************************************************************
   [DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release 
   will default to using the discovered platform python for this host. See https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be 
   removed in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
   ok: [ubuntu]
   ok: [centos7]
   
   TASK [Print OS] ********************************************************************************************************************************************************************************************
   ok: [centos7] => {
       "msg": "CentOS"
   }
   ok: [ubuntu] => {
       "msg": "Ubuntu"
   }
   
   TASK [Print fact] ******************************************************************************************************************************************************************************************
   ok: [centos7] => {
       "msg": "el default fact"
   }
   ok: [ubuntu] => {
       "msg": "deb default fact"
   }
   
   PLAY RECAP *************************************************************************************************************************************************************************************************
   centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

    ```
7. При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.
   ```bash
   $ cat group_vars/{deb,el}/*
   $ANSIBLE_VAULT;1.1;AES256
   33326465373730303763383330653765613561373265383538663662636435366336376430373132
   6430343732333632613663353262333832326636356662630a616364616630373933663930646634
   30366138633037316237353239383135653738396637616236633233393038633965633634316132
   6563643432303431380a626435363838626334353232313661343033323031343861303331616634
   37306337383539366662643636376631313630323035356365313666633139343731313638383564
   3336636566666433616636636166656436386663626437663536
   $ANSIBLE_VAULT;1.1;AES256
   37353838373361653866353064353032333062343635623733313464353936303932336232666361
   3534633663383532383363326136346434393936383536340a666635333736383366663265323865
   35313430666564626633666166346363316264636438386164376166663236323531373839653462
   3934393031656566640a306561616233613232636634373837336432623933636239393133653766
   36633464343735653461343964633662666437346531616432353133393862366230626538393533
   3964626233313561346532613239326238356333653733316130

   ```
8. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.
   ```bash
   $ ansible-playbook site.yml -i inventory/prod.yml --vault-password-file vault_password.txt
   
   PLAY [Print os facts] **************************************************************************************************************************************************************************************

   TASK [Gathering Facts] *************************************************************************************************************************************************************************************
   [DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release 
   will default to using the discovered platform python for this host. See https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be 
   removed in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
   ok: [ubuntu]
   ok: [centos7]
   
   TASK [Print OS] ********************************************************************************************************************************************************************************************
   ok: [centos7] => {
       "msg": "CentOS"
   }
   ok: [ubuntu] => {
       "msg": "Ubuntu"
   }
   
   TASK [Print fact] ******************************************************************************************************************************************************************************************
   ok: [centos7] => {
       "msg": "el default fact"
   }
   ok: [ubuntu] => {
       "msg": "deb default fact"
   }
   
   PLAY RECAP *************************************************************************************************************************************************************************************************
   centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   ```
9. Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.
   ```bash
   $ ansible-doc -t connection -l
   ansible.netcommon.httpapi      Use httpapi to run command on network appliances                                                                                                                        
   ansible.netcommon.libssh       (Tech preview) Run tasks using libssh for ssh connection                                                                                                                
   containers.podman.buildah      Interact with an existing buildah container                                                                                                                             
   containers.podman.podman       Interact with an existing podman container                                                                                                                              
   local                          execute on controller                        
      ```    
   Для подключения к ноде-контроллеру нужно использовать local  
      

10. В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.
    ```yaml
    $ cat inventory/prod.yml
    ---
      el:
        hosts:
          centos7:
            ansible_connection: docker
      deb:
        hosts:
          ubuntu:
            ansible_connection: docker
      local:
        hosts:
          localhost:
            ansible_connection: local
    ```
11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь что факты `some_fact` для каждого из хостов определены из верных `group_vars`.
    ```bash
    $ ansible-playbook site.yml -i inventory/prod.yml --vault-password-file vault_password.txt
      PLAY [Print os facts] **************************************************************************************************************************************************************************************

      TASK [Gathering Facts] *************************************************************************************************************************************************************************************
      ok: [localhost]
      [DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release 
      will default to using the discovered platform python for this host. See https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information. This feature will be 
      removed in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
      ok: [ubuntu]
      ok: [centos7]
      
      TASK [Print OS] ********************************************************************************************************************************************************************************************
      ok: [localhost] => {
          "msg": "Ubuntu"
      }
      ok: [centos7] => {
          "msg": "CentOS"
      }
      ok: [ubuntu] => {
          "msg": "Ubuntu"
      }
      
      TASK [Print fact] ******************************************************************************************************************************************************************************************
      ok: [localhost] => {
          "msg": "all default fact"
      }
      ok: [centos7] => {
          "msg": "el default fact"
      }
      ok: [ubuntu] => {
          "msg": "deb default fact"
      }
      
      PLAY RECAP *************************************************************************************************************************************************************************************************
      centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
      localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
      ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
    ```
