Role Description
=========

Роль `deploy_dhcp`.

Разворачивает отказоустойчивый DHCP сервер.


> [!WARNING]
> Тестировалось на:
> - RedOS 8


> [!NOTE]
> https://redos.red-soft.ru/base/redos-8_0/8_0-network/8_0-dhcp/8_0-failover-dhcp/


## Использование роли

Установить или развернуть ОС из шаблона ВМ.

Настроить на хосте IP адрес.

Обновить ОС до нужной версии.

Перезагрузить хост.

Запустить плэйбук.


Requirements
------------



Role Variables
--------------



Dependencies
------------



Example Playbook
----------------

File inventory:
```yaml
dhcp_servers:
  children:
    ostp:
      hosts:
        ostp-dhcp-test-p:
          ansible_host: 192.168.0.138
          g_primary_node: true
          g_secondary_node_ip: 192.168.0.139
        ostp-dhcp-test-s:
          ansible_host: 192.168.0.139
          g_secondary_node_ip: 192.168.0.138
    ekb:
      hosts:
        ekb-dhcp-test-p:
          ansible_host: 192.168.0.141
          g_primary_node: true
          g_secondary_node_ip: 192.168.0.142
        ekb-dhcp-test-s:
          ansible_host: 192.168.0.142
          g_secondary_node_ip: 192.168.0.141
```

File playbook:
```yaml
- name: Deploy DHCP
  hosts: dhcp_servers
  gather_facts: true
  become: true
  roles:
    - configure_host_after_deployment
    - deploy_dhcp
```

File variables:

some_file.yml
```YAML
ddhcp_shared_network: office
ddhcp_domain_search:
  - rt-dc.local
  - rt-dc.ru
ddhcp_netmask: 255.255.255.0
ddhcp_dns:
  - 192.168.0.1
  - 192.168.0.2
ddhcp_network: 192.168.0.0
ddhcp_gateway: 192.168.0.1
ddhcp_pool: "192.168.0.10 192.168.0.49"
```

Run:
```bash
$ ansible-playbook -i inventory/dhcp_servers.yml deploy_dhcp.yml --ask-pass -u root --ask-vault-pass -l "ostp-dhcp-test-p"
$ ansible-playbook -i inventory/dhcp_servers.yml deploy_dhcp.yml --ask-become-pass --ask-pass -u user_name --ask-vault-pass -l "ostp-dhcp-test-p" -e "ddhcp_shared_network=office"
```


License
-------
