Role Name
=========

Add sudo rights to the user.
Добавить права sudo пользователю.

Requirements
------------

Add to file ansible.cfg
Добавить в файл ansible.cfg
---
    [defaults]
    stdout_callback=community.general.diy

    [callback_diy]
    # Accept on_skipped_msg or ansible_callback_diy_runner_on_skipped_msg as input vars
    # If neither are supplied, omit the option
    runner_on_skipped_msg="{{ on_skipped_msg | default(ansible_callback_diy_runner_on_skipped_msg) | default(omit) }}"
    runner_on_skipped_msg_color="{{ on_skipped_msg_color | default(ansible_callback_diy_runner_on_skipped_msg_color) | default('cyan') }}"
...

Role Variables
--------------

sudo_users    - list of users for whom rights are configured (список пользователей, для которых настраиваются права). Mandatory, Default: [];
sudo_commands - list of commands allowed to the user (список команд, разрешенных пользователю). Mandatory, Default: [];
run_as        - from which user or group to run commands (от какого пользователя или группы запускать команды). Default: root;
nopasswd      - don't ask for password for sudo (не спрашивать пароль для sudo). Default: false.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
---
    - hosts: all
      gather_facts: false
      become: yes
      roles:
        - role: add_rule_sudoers
...

ansible-playbook -i inventory.yml add_sudo.yml --extra-vars '{"sudo_users":["user1","user2"], "sudo_commands":["/usr/bin/cat","/usr/bin/ls"], "run_as":ALL, "nopasswd":true}'
ansible-playbook -i inventory.yml add_sudo.yml --extra-vars '{"sudo_users":["user1","user2"], "sudo_commands":["/usr/bin/cat","/usr/bin/ls"]}'
ansible-playbook -i inventory.yml add_sudo.yml -e '{"sudo_users":["user1","user2"], "sudo_commands":["/usr/bin/cat","/usr/bin/ls"]}' --connection=local --check

License
-------



Author Information
------------------

https://github.com/sdngit
