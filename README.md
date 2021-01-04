# Ansible Role: postrgresql

Installs and configures PostgreSQL server on Debian servers.

## Requirements

No special requirements; note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

    - hosts: database
      roles:
        - role: wreiner.postgresql
          become: yes

## Role Variables

### Needed variables

```
postgresql__version: 13

postgresql__databases_roles:
  db1:
    dbname: "ttrss"
    roles:
      role1:
        rolename: role_ttrss_default
        privileges: "ALL"
        type: "default_privs"
        objs: ALL_DEFAULT
      role2:
        rolename: role_ttrss_readonly
        privileges: "SELECT"
        type: "default_privs"
        objs: TABLES,SEQUENCES
  db2:
    dbname: "wwwwreinerat"
    roles:
      role1:
        rolename: role_wwwwreinerat_default
        privileges: "ALL"
        type: "default_privs"
        objs: ALL_DEFAULT

postgresql__users:
  user1:
    username: "u_ttrss"
    password: "abc"
    role:
      - "role_ttrss_default"
      - "role_wwwwreinerat_default"
```

### Additional variables needed to enable SSL

```
postgresql__additional_conf_dir: "/etc/postgresql/{{ postgresql__version }}/main/conf.d"

postgresql__ssl: true
postgresql__ssl_ca_file: "/etc/postgresql/{{ postgresql__version }}/main/ssl/wreiner.at-ca.pem"
postgresql__ssl_cert_file: "/etc/postgresql/{{ postgresql__version }}/main/ssl/testdeb.wreiner.at-cert.pem"
postgresql__ssl_key_file: "/etc/postgresql/{{ postgresql__version }}/main/ssl/testdeb.wreiner.at-key.pem"
postgresql__ssl_dh_params_file: "/etc/postgresql/{{ postgresql__version }}/main/ssl/dh_params"
```
