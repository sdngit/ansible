Role Description
=========

Роль `get_host_info`.

Собирает информацию на хосте:\
- Имя хоста
- IP
- ОС
- Установленные пакеты
- CPU
- RAM
- Объем дисков

На хосте, на котором запускается роль Ansible, будет создан файл с собраной информацией.


> [!CAUTION]
> Тестировалось на:\
> - RedOS
> - Debian
> - Alt Linux


## Использование роли

Запустить плэйбук.


Requirements
------------



Role Variables
--------------

| **Variable**             | **Default value** | **Description**                                                                                       |
| ------------------------ | ----------------- | ----------------------------------------------------------------------------------------------------- |
| ghi_package_name_regex | "^postgres"       | Название пакета, для которого хотим получить информацию. Указывается регулярное выражение для поиска. |
| ghi_file_info_path     | ./file_info.csv   | Файл .csv, в котором собрана найденная информация. Сохраняется на хосте, откуда запускается Ansible.  |


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
ghi_package_name_regex: "^postgres"
ghi_file_info_path: ~/file_info.csv
```

Run:
```bash
$ ansible-playbook -i inventory/ get_info_about_installed_packages.yml --ask-pass -u user_name
$ ansible-playbook get_info_about_installed_packages.yml -e '{"ghi_package_name_regex": ["^docker"]}'
$ ansible-playbook get_info_about_installed_packages.yml -e '{"ghi_package_name_regex": ["^docker"],"ghi_file_info_path": "./file_info_docker.csv"}'
```


License
-------
