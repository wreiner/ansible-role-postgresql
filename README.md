# postrgresql

## Variables

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
