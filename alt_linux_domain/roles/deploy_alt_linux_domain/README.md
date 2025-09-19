Role Description
=========

Роль `deploy_alt_linux_domain`.

Разворачивает Samba домен на Alt Linux 11.
При запуске плэйбука будут запрошены логин администратора (по умолчанию "administrator") и пароль.
При создании нового домена автоматически создается учетная запись с именем "administrator". Изменить это поведение нельзя.


Requirements
------------



Role Variables
--------------

Переменные, на которые стоит обратить внимание:
| **Variable**               | **Default value** | **Description**                                                                                                                                                                                                                                                   |
| :------------------------- | :---------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dald_primary_dc            | false             | Указывает является ли разворачиваемый контроллер домена первым (Primary) или дополнительным (Secondary).<br><br>true - primary.<br>Будет создан новый домен.<br><br>false - secondary.<br>Хост будет добавлен в существующий домен как дополнительный контроллер. |
| dald_domain_function_level | 2016              | Уровень функциональности домена и леса: 2000, 2003, 2008, 2008_R2, или 2016.                                                                                                                                                                                      |
| dald_base_schema           | 2019              | Версия базовой схемы домена.                                                                                                                                                                                                                                      |
| dald_primary_dc_ip         |                   | IP адрес уже развернутого контроллера домена. Требуется при добавлении нового контроллера.                                                                                                                                                                        |


Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
    - hosts: all
      gather_facts: false
      become: yes
      roles:
        - role: add_rule_sudoers
```

File variables:

some_file.yml
```YAML
    sudo_users:
        - user1
        - user2
    sudo_commands:
        - '/usr/bin/cat'
        - '/usr/bin/ls'
    run_as: "ALL"
    nopasswd: true
```
some_file.json
```JSON
    {
        "sudo_users": [
            "user1",
            "user2"
        ],
        "sudo_commands": [
            "/usr/bin/cat",
            "/usr/bin/ls"
        ],
        "run_as": "ALL",
        "nopasswd": true
    }
```

Run:
```bash
    $ ansible-playbook -i inventory.yml add_sudo.yml --extra-vars '{"sudo_users":["user1","user2"], "sudo_commands":["/usr/bin/cat","/usr/bin/ls"], "run_as":ALL, "nopasswd":true}'
    $ ansible-playbook -i inventory.yml add_sudo.yml --extra-vars '{"sudo_users":["user1","user2"], "sudo_commands":["/usr/bin/cat","/usr/bin/ls"]}'

    # Vars from a JSON or YAML file
    $ ansible-playbook -i inventory.yml add_sudo.yml --extra-vars "@some_file.yml"
    $ ansible-playbook -i inventory.yml add_sudo.yml --extra-vars "@some_file.json"

    # Local test
    $ ansible-playbook -i inventory.yml add_sudo.yml -e '{"sudo_users":["user1","user2"], "sudo_commands":["/usr/bin/cat","/usr/bin/ls"]}' --connection=local --check
```


License
-------



Author Information
------------------

https://github.com/sdngit
