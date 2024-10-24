Role Name
=========

Add sudo rights to the user. \
Добавить пользователю права sudo.


Requirements
------------

Add to file ansible.cfg \
Добавить в файл ansible.cfg
```ini
    [defaults]
    stdout_callback=community.general.diy

    [callback_diy]
    # Accept on_skipped_msg or ansible_callback_diy_runner_on_skipped_msg as input vars
    # If neither are supplied, omit the option
    runner_on_skipped_msg="{{ on_skipped_msg | default(ansible_callback_diy_runner_on_skipped_msg) | default(omit) }}"
    runner_on_skipped_msg_color="{{ on_skipped_msg_color | default(ansible_callback_diy_runner_on_skipped_msg_color) | default('cyan') }}"
```

Role Variables
--------------

- `sudo_users` - list of users for whom rights are configured (список пользователей, для которых настраиваются права). \
  Mandatory. \
  Default: [];
- `sudo_commands` - list of commands allowed to the user (список команд, разрешенных пользователю). \
  Mandatory. \
  Default: [];
- `run_as` - from which user or group to run commands (от какого пользователя или группы запускать команды). \
  Default: root;
- `nopasswd` - don't ask for password for sudo (не спрашивать пароль для sudo). \
  Default: false.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.


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
