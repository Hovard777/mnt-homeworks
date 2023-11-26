# lighthouse-role for Centos 7
=========

Роль устанавливает Lighthouse на CentOS 7.


Dependencies
------------
NGINX

Example Playbook
----------------
```yaml
- name: Install lighthouse
  hosts: lighthouse
  pre_tasks:
    - name: Lighthouse | install dependencies
      become: true
      ansible.builtin.yum:
        name: git
        state: present
  roles: 
    - lighthouse
```

