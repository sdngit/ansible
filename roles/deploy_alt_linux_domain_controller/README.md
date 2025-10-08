Role Description
=========

Роль `deploy_alt_linux_domain_controller`.

Разворачивает новый Samba домен на Alt Linux 11 или добавляет в существующий.\
При запуске плэйбука будут запрошены логин администратора (по умолчанию "administrator") и пароль.\
При создании нового домена автоматически создается учетная запись с именем "administrator". Изменить это поведение нельзя.


> [!CAUTION]
> Тестировалось только на Alt Linux 11.


> [!NOTE]
> https://docs.altlinux.org/ru-RU/alt-domain/11.0/html/alt-domain/ \
> https://www.altlinux.org/ActiveDirectory/DC


> [!CAUTION]
> Смена владельца FSMO ролей. \
> Чтобы работала репликация между контроллерами домена необходимо при смене владельца ролей FSMO скопировать данные из файлов на нового владельца:
> - /etc/cron.d/sysvol
> - /etc/cron.d/idmap
> - /root/.ssh/known_hosts
> - /root/.ssh/authorized_keys
> Либо запустить для SDC playbook с тэгом "daldc_configure_replication", не забыв указать нового PDC.


Назначет хосту имя в соответствии с тем, как он указан в `inventory`. \
Заменяет `SSH ключи` и `machine-id`. \
Настраивает DNS. \
Создает новый домен или добавляет к существующему. \
Добавляет задания в `cron` для репликации `SysVol` и `idmap.ldb`.


## Использование роли

Установить или развернуть ОС из шаблона ВМ.

Настроить на хосте IP адрес и временный DNS.

Если задать имя хоста руками, то роль не выполнит некоторые изменения и их придеться выполнить самостоятельно:
  - изменить имя хоста в файле /etc/sysconfig/network;
  - изменить имя хоста в файле /etc/alterator/dhcp/general;
  - изменить имя хоста в файле /etc/alterator/dhcp6/general;
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

| **Variable**               | **Default value**                                                                                                 | **Description**                                                                                                                                                                                                                                                   |
| :------------------------- | :---------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| daldc_create_domain         | false                                                                                                             | Указывает является ли разворачиваемый контроллер домена первым (Primary) или дополнительным (Secondary).<br><br>true - primary.<br>Будет создан новый домен.<br><br>false - secondary.<br>Хост будет добавлен в существующий домен как дополнительный контроллер. |
| daldc_domain_function_level | 2016                                                                                                              | Уровень функциональности домена и леса: 2000, 2003, 2008, 2008_R2, или 2016.                                                                                                                                                                                      |
| daldc_base_schema           | 2019                                                                                                              | Версия базовой схемы домена.                                                                                                                                                                                                                                      |
| daldc_ip_primary_dc         |192.168.0.51                                                                                                       | IP адрес основного контроллера домена (владельца FSMO ролей). Требуется при добавлении нового контроллера.                                                                                                                                                                        |
| daldc_net_ifaces            | eth0                                                                                                              | Сетевой интерфейс. Требуется для настройки DNS.                                                                                                                                                                                                                   |
| daldc_dns_forward_servers   | 1.1.1.1<br>8.8.8.8                                                                                                | Адреса DNS серверов для пересылки.                                                                                                                                                                                                                                |
| daldc_allowed_dns_clients   | 192.168.0.0/24                                                                                                    | Список адресов, откуда разрешены DNS запросы.                                                                                                                                                                                                                     |


Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
- hosts: alt_dc
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
    - configure_ntp_chrony
    - deploy_alt_linux_domain_controller
```

File variables:

some_file.yml
```YAML
daldc_create_domain: true
daldc_domain_function_level: 2016
daldc_net_ifaces: eth0
daldc_dns_forward_servers:
  - 192.168.0.1
  - 8.8.8.8
daldc_allowed_dns_clients:
  - 192.168.0.0/24
  - 192.168.1.0/24
  - 192.168.3.0/24
```

Run:
```bash
# Для создания нового домена
$ ansible-playbook -i ./inventory.yml deploy_alt_linux_domain_controller.yml --ask-pass -u root -l "dc1.test.alt" -e "daldc_create_domain=true"

# Для ввода дополнительного контроллера домена в существующий домен
$ ansible-playbook -i ./inventory.yml deploy_alt_linux_domain_controller.yml --ask-pass -u root -l "dc2.test.alt"

# Передать переменные через файл
$ ansible-playbook -i ./inventory.yml deploy_alt_linux_domain_controller.yml --ask-pass -u root -l "dc1.test.alt" -e "@some_file.yml"
```


License
-------



Author Information
------------------

https://github.com/sdngit
