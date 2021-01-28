# Ansible Role: postgresql

Installs and configures PostgreSQL server on Debian servers.

## Requirements

No special requirements; note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

    - hosts: database
      roles:
        - role: wreiner.postgresql
          become: yes

## Role Variables

The PostgreSQL version which should be installed must be defined.

```
postgresql__version: 13
```

Also the main config file must be defined.

```
postgresql__main_config: "/etc/postgresql/{{ postgresql__version }}/main/postgresql.conf"
```

In the default configuration PostgreSQL only listens on localhost. To change that behaviour set:

```
postgresql__listen_addresses: "127.0.0.1,{{ mgmt_ipaddress }}"
```

Databases are created with one or more roles.

```
postgresql__databases_roles:
  db1:
    dbname: "ttrss"
    roles:
      role1:
        rolename: role_ttrss_default
        privileges: "ALL"
        type: "database"
      role2:
        rolename: role_ttrss_readonly
        privileges: "SELECT"
        type: "default_privs"
        objs: TABLES,SEQUENCES
  db2:
    dbname: "nextcloud"
    roles:
      role1:
        rolename: role_nextcloud_default
        privileges: "ALL"
        type: "database"
```

Options for roles include:

- `rolename` (required)
- `privileges` (required)
- `type` (required)
- `objs` (optional)

Users are added to the roles created earlier.

```
postgresql__users:
  user1:
    username: "u_multiple_roles"
    password: "abc"
    role:
      - "role_ttrss_default"
      - "role_nextcloud_default"
```

### Additional variables needed to enable SSL

```
postgresql__additional_conf_dir: "/etc/postgresql/{{ postgresql__version }}/main/conf.d"

postgresql__ssl: true
postgresql__ssl_ca_file: "/etc/postgresql/{{ postgresql__version }}/main/ssl/example.com-ca.pem"
postgresql__ssl_cert_file: "/etc/postgresql/{{ postgresql__version }}/main/ssl/host.example.com-cert.pem"
postgresql__ssl_key_file: "/etc/postgresql/{{ postgresql__version }}/main/ssl/host.example.com-key.pem"
postgresql__ssl_dh_params_file: "/etc/postgresql/{{ postgresql__version }}/main/ssl/dh_params"
```

The ownership and permissions of the certficate and the key will be changed so the files can be deployed by another role before the postgres user exists.
