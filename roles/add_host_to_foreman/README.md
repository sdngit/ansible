Role Description
=========

Роль `add_host_to_foreman`.

Устанавливает необходимые пакеты и добавляет хост в Foreman.\
При запуске плэйбука будут запрошены логин и пароль пользователя в Foreman.\
По умолчанию хост настраивается в режиме Pull.\

Для корректной работы требуется открыть порты TCP:\
80   - установка сертификатов Foreman;\
443  - регистрация хоста;\
1883 - работа хоста в режиме Pull;\
9090 - обращение к Smart Proxy.


> [!CAUTION]
> Тестировалось на RedOS 8.


## Использование роли

Запустить плэйбук с указанием `ahtf_activation_keys` (ключи активации).


Requirements
------------



Role Variables
--------------

Переменные, на которые стоит обратить внимание:

`ahtf_foreman_server` - сервер Foreman или Smart Proxy, через который работает хост-клиент.\
    default: `frmn-prd-mng01.rt-dc.local`\

`ahtf_activation_keys` - ключи активации, используемые для развертывания. Может быть список, разделенный запятыми.\
    default: `ak-redos80-dev`


Dependencies
------------



Example Playbook
----------------

File playbook:
```yaml
- name: Add a host to Foreman-Katello
  hosts: all
  gather_facts: true
  become: true
  vars_prompt:
    - name: cli_foreman_username
      prompt: Enter Foreman username
      private: false

    - name: cli_foreman_password
      prompt: Enter password
      unsafe: true
      private: true

    - name: cli_foreman_password_retype
      prompt: Re-type password
      unsafe: true
      private: true

  pre_tasks:
    - name: Check the variable cli_foreman_password
      ansible.builtin.assert:
        that:
          - cli_foreman_password | length > 0
        fail_msg: The variable is empty!
        quiet: true

    - name: Checking password matches
      ansible.builtin.assert:
        that:
          - cli_foreman_password == cli_foreman_password_retype
        fail_msg: Sorry, passwords do not match.
        quiet: true

  roles:
    - add_host_to_foreman
```

Run:
```bash
# Добавить хост-клиент в Foreman
$ ansible-playbook add_host_to_foreman.yml -l "ones-tst-db-pg01,ones-tst-lic"
$ ansible-playbook add_host_to_foreman.yml -e "ahtf_activation_keys=ak-redos80-test,ak-pgpro-std-18-redos8.0-test" -l "ones-tst-db-pg01,ones-tst-lic"
```
