Role Description
=========

Роль `deploy_freeradius`.

Разворачивает `FreeRadius` сервер и настраивает доменную авторизацию через `winbind`.


> [!IMPORTANT]
> Тестировалось на RedOS 8.


Настраивает DNS. \
Назначет хосту имя в соответствии с тем, как он указан в `inventory`. \
Заменяет `SSH ключи` и `machine-id`. \
Заводит хост в домен. \
Настраивает FreeRadius.


## Использование роли

Установить или развернуть ОС из шаблона ВМ.

Настроить на хосте IP адрес и временный DNS.

Если задать имя хоста руками, то роль не выполнит некоторые изменения и их придеться выполнить самостоятельно:
  - заменить SSH ключи хоста;
  - заменить machine-id.

Обновить ОС до нужной версии.

Перезагрузить хост.

Запустить плэйбук.


Requirements
------------



Role Variables
--------------

Переменные, на которые стоит обратить внимание:

| **Variable**           | **Default value**                                                                                                                                                                                                                                                                                    | **Description**                                         |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| dfr_net_ifaces         | eth0                                                                                                                                                                                                                                                                                                 | Сетевой интерфейс требуется для настройки DNS.          |
| dfr_dns4_servers       | 192.168.0.51<br>192.168.0.52<br>192.168.0.53                                                                                                                                                                                                                                                         | Сервера для настройки DNS на хосте.                     |
| dfr_domain_controllers | dc1.altdomain.test<br>dc2.altdomain.test<br>dc3.altdomain.test                                                                                                                                                                                                                                       | Контроллеры домена.                                     |
| dfr_radius_clients     | Network_test:<br>    ipaddr: 192.168.0.0/24<br>    secret: "{{ dfr_vault_secret_radius_clients['Network_test'] }}"<br>    comment: "network test1"<br>  WiFi_test:<br>    ipaddr: 192.168.1.0/24<br>    secret: "{{ dfr_vault_secret_radius_clients['WiFi_test'] }}"<br>    comment: "network test2" | Клиенты, которым разрешена аутентификация через radius. |


Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
- name: Deploy freeradius
  hosts: all
  gather_facts: true
  become: yes
  vars_prompt:
    - name: cli_admin_AD_username
      prompt: Enter the administrator AD username
      private: false
      default: "administrator"

    - name: cli_admin_AD_pass
      prompt: Enter the administrator AD password
      unsafe: true
      private: true

    - name: cli_admin_AD_pass_retype
      prompt: Re-type password
      unsafe: true
      private: true

  roles:
    - configure_host_after_deployment
    - deploy_freeradius
```

File variables:

some_file.yml
```YAML
dfr_domain_controllers:
  - dc1.altdomain.test
  - dc2.altdomain.test
  - dc3.altdomain.test
```

Run:
```bash
$ ansible-playbook --ask-vault-pass -i inventory/freeradius.yml deploy_freeradius.yml --ask-pass -u root -l "freeradius.altdomain.test"
$ ansible-playbook --vault-id test_deploy_freeradius@prompt -i inventory/freeradius.yml deploy_freeradius.yml --ask-pass -u root -l "freeradius.altdomain.test"
$ ansible-playbook --vault-id test_deploy_freeradius@prompt -i inventory/freeradius.yml deploy_freeradius.yml --ask-pass -u root -l "freeradius.altdomain.test" -e "@some_file.yml"
```


License
-------
