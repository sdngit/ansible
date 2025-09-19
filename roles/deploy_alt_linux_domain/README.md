Role Description
=========

Роль `deploy_alt_linux_domain`.

Разворачивает Samba домен на Alt Linux 11.\
При запуске плэйбука будут запрошены логин администратора (по умолчанию "administrator") и пароль.\
При создании нового домена автоматически создается учетная запись с именем "administrator". Изменить это поведение нельзя.


Requirements
------------



Role Variables
--------------

Переменные, на которые стоит обратить внимание:

| **Variable**               | **Default value**                                                                                                 | **Description**                                                                                                                                                                                                                                                   |
| :------------------------- | :---------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dald_primary_dc            | false                                                                                                             | Указывает является ли разворачиваемый контроллер домена первым (Primary) или дополнительным (Secondary).<br><br>true - primary.<br>Будет создан новый домен.<br><br>false - secondary.<br>Хост будет добавлен в существующий домен как дополнительный контроллер. |
| dald_domain_function_level | 2016                                                                                                              | Уровень функциональности домена и леса: 2000, 2003, 2008, 2008_R2, или 2016.                                                                                                                                                                                      |
| dald_base_schema           | 2019                                                                                                              | Версия базовой схемы домена.                                                                                                                                                                                                                                      |
| dald_primary_dc_ip         |                                                                                                                   | IP адрес уже развернутого контроллера домена. Требуется при добавлении нового контроллера.                                                                                                                                                                        |
| dald_ntp_servers           | ntp.msk-ix.ru<br>ntp1.vniiftri.ru<br>ntp2.vniiftri.ru<br>ntp3.vniiftri.ru<br>ntp4.vniiftri.ru<br>ntp5.vniiftri.ru | Список NTP серверов для настройки `chrony`.                                                                                                                                                                                                                       |
| dald_allowed_ntp_clients   | 192.168.0.0/16                                                                                                    | Список адресов, откуда разрешено обращаться к `chrony`.                                                                                                                                                                                                           |
| dald_net_ifaces            | eth0                                                                                                              | Сетевой интерфейс. Требуется для настройки DNS.                                                                                                                                                                                                                   |
| dald_dns_forward_servers   | 1.1.1.1<br>8.8.8.8                                                                                                | Адреса DNS серверов для пересылки.                                                                                                                                                                                                                                |
| dald_allowed_dns_clients   | 192.168.0.0/24                                                                                                    | Список адресов, откуда разрешены DNS запросы.                                                                                                                                                                                                                     |


Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
- hosts: all
  become: true
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
    - deploy_alt_linux_domain
```

File variables:

some_file.yml
```YAML
dald_primary_dc: true
dald_domain_function_level: 2016
dald_net_ifaces: eth0
dald_dns_forward_servers:
    - 192.168.0.1
    - 8.8.8.8
dald_allowed_dns_clients:
    - 192.168.0.0/24
    - 192.168.1.0/24
    - 192.168.3.0/24
```

Run:
```bash
# Для создания нового домена
$ ansible-playbook -i ./inventory.yml deploy_alt_linux_domain.yml --ask-pass -u root -l "dc1.test.alt" -e "dald_primary_dc=true"

# Для ввода дополнительного контроллера домена в существующий домен
$ ansible-playbook -i ./inventory.yml deploy_alt_linux_domain.yml --ask-pass -u root -l "dc2.test.alt"

# Передать переменные через файл
$ ansible-playbook -i ./inventory.yml deploy_alt_linux_domain.yml --ask-pass -u root -l "dc1.test.alt" -e "@some_file.yml"
```


License
-------



Author Information
------------------

https://github.com/sdngit
