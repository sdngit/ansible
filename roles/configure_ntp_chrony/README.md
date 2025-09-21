Role Description
=========

Роль `configure_ntp_chrony`.

Настроить синхронизацию времени с помощью `Chrony`.

1. Удаляет пакет NTP.
2. Отключает `systemd-timesyncd`.
3. Устанавливает и настраивает `chrony`.


Requirements
------------



Role Variables
--------------

| **Variable**              | **Default value**                                                                                                 | **Description**                                                          |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| cntpc_ntp_servers         | ntp.msk-ix.ru<br>ntp1.vniiftri.ru<br>ntp2.vniiftri.ru<br>ntp3.vniiftri.ru<br>ntp4.vniiftri.ru<br>ntp5.vniiftri.ru | Список серверов точного времени для синхронизации.                       |
| cntpc_allowed_dns_clients | 192.168.0.0/16<br>10.0.0.0/8                                                                                      | Разрешённые сети для DNS-запросов. В том числе для рекурсивных запросов. |


Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
- name: Configure NTP
  hosts: all
  gather_facts: false
  become: yes
  roles:
    - configure_ntp_chrony
```

File variables:

some_file.yml
```YAML
cntpc_ntp_servers:
  - ntp.msk-ix.ru
  - ntp1.vniiftri.ru
  - ntp2.vniiftri.ru
  - ntp3.vniiftri.ru
  - ntp4.vniiftri.ru
  - ntp5.vniiftri.ru

cntpc_allowed_dns_clients:
  - 192.168.0.0/16
  - 10.0.0.0/8
```

Run:
```bash
$ ansible-playbook -i ./inventory.yml configure_ntp_chrony.yml --ask-pass -u root -l "dc1.test.alt"
$ ansible-playbook -i ./inventory.yml configure_ntp_chrony.yml --ask-pass -u root -l "dc1.test.alt" -e "@some_file.yml"
```


License
-------



Author Information
------------------

https://github.com/sdngit
