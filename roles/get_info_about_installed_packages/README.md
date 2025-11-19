Role Description
=========

Роль `get_info_about_installed_packages`.

Проверяет, установлен ли указанный пакет на хосте.\
На хосте, на котором запускается роль Ansible, будет создан файл с найденной информацией.


> [!WARNING]
> Тестировалось на:
> - RedOS 8


## Использование роли

Запустить плэйбук.


Requirements
------------



Role Variables
--------------

| **Variable**             | **Default value** | **Description**                                                                                       |
| ------------------------ | ----------------- | ----------------------------------------------------------------------------------------------------- |
| giaip_package_name_regex | "^postgres"       | Название пакета, для которого хотим получить информацию. Указывается регулярное выражение для поиска. |
| giaip_file_info_path     | ./file_info.csv   | Файл .csv, в котором собрана найденная информация. Сохраняется на хосте, откуда запускается Ansible.  |


Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
- name: Get info about installed packages
  hosts: all
  gather_facts: true
  become: false
  roles:
    - get_info_about_installed_packages
```

File variables:

some_file.yml
```YAML
giaip_package_name_regex: "^postgres"
giaip_file_info_path: ./file_info.csv
```

Run:
```bash
$ ansible-playbook -i inventory/ get_info_about_installed_packages.yml --ask-pass -u root
```


License
-------
