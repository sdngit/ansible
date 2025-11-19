Role Description
=========

Роль `configure_host_after_deployment`.

Настраивает `DNS`. \
Назначет хосту имя в соответствии с тем, как он указан в `inventory`. \
Если текущий `hostname` хоста отличается от указанного в `inventory` так же будут заменены `SSH ключи` и `machine-id`.

> [!IMPORTANT]
> Тестировалось на:
> RedOS 8


## Использование роли

Установить или развернуть ОС из шаблона ВМ.

Настроить на хосте IP адрес.

Если задать имя хоста руками, то роль не выполнит некоторые изменения и их придеться выполнить самостоятельно:
  - заменить SSH ключи хоста;
  - заменить machine-id.

Перезагрузить хост.

Запустить плэйбук.


Requirements
------------



Role Variables
--------------

| **Variable**      | **Default value**          | **Description**                                                                                                                                      |
| ----------------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| chad_net_ifaces   | eth0                       | Интерфейс, на котором настраивается DNS.                                                                                                             |
| chad_dns4_servers | 192.168.0.1<br>192.168.0.2 | DNS сервера.                                                                                                                                         |
| chad_domain_name  | Значение из `inventory`    | Имя домена, которое используется в `Search domains`.<br>Если в `inventory` указан FQDN, то значение берется из него. Если нет, значение будет `'.'`. |


Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
- name: Configure host after deployment
  hosts: all
  gather_facts: false
  become: true
  roles:
    - configure_host_after_deployment
```

File variables:

some_file.yml
```YAML
chad_net_ifaces: eth0

chad_dns4_servers:
  - 192.168.0.1
  - 192.168.0.2

chad_domain_name: domain.test
```

Run:
```bash
$ ansible-playbook -i inventory/dhcp_servers.yml configure_host_after_deployment.yml --ask-pass -u root -l "dhcp-test" --ssh-common-args '-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
$ ansible-playbook -i inventory/dhcp_servers.yml configure_host_after_deployment.yml --ask-become-pass --ask-pass -u user_name -l "dhcp-test" --ssh-common-args '-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
```


License
-------
