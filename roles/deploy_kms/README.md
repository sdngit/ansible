Role Description
=========

Роль `deploy_kms`.

Развернуть KMS.


> [!IMPORTANT]
> Тестировалось на:
> - RedOS 7
> - RedOS 8


## Использование роли

Запустить плэйбук `deploy_kms.yml`.


Requirements
------------



Role Variables
--------------




Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
- name: Deploy KMS
  hosts: all
  gather_facts: false
  become: true
  roles:
    - deploy_kms
```

File variables:

some_file.yml
```YAML

```

Run:
```bash
$ ansible-playbook -i inventory/kms.yml deploy_kms.yml --ask-pass -u root -l "kms-server"
```


License
-------
