Role Name
=========

Install packages for Linux.


Requirements
------------



Role Variables
--------------

- `packages` - list of packages to install. \
  Mandatory. \
  Default: [].

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.


Example Playbook
----------------

File playbook:
```yaml
    - name: Install packages
      hosts: all
      gather_facts: false
      become: yes
      roles:
        - role: install_packages
```

File variables:

some_file.yml
```YAML
    packages:
      - htop
      - nftables
```
some_file.json
```JSON
    {
        "packages": [
            "htop",
            "nftables"
        ]
    }
```

Run:
```bash
    $ ansible-playbook -i inventory.yml install_packages.yml --extra-vars '{"packages":["htop","nftables"]}'

    # Vars from a JSON or YAML file
    $ ansible-playbook -i inventory.yml install_packages.yml -e "@some_file.yml"
    $ ansible-playbook -i inventory.yml install_packages.yml -e "@some_file.json"
```

License
-------



Author Information
------------------

https://github.com/sdngit
