# Role Name

Postgresql role for Ubuntu

## Requirements

## Role Variables

defaults for postgresql-role:

---

pgsql_version: 15

pgsql_support_packages:

- gnupg
- python3-psycopg2

pgsql_packages:

- "postgresql-{{ pgsql_version }}"
- "postgresql-contrib-{{ pgsql_version }}"

pgsql_repo_key: https://www.postgresql.org/media/keys/ACCC4CF8.asc

pgsql_repo: "deb http://apt.postgresql.org/pub/repos/apt/ {{ ansible_distribution_release }}-pgdg main"
pgsql_repo_filename: pgdg

pgsql_postgres_password: postgres

pgsql_data_dir: "/var/lib/postgresql/{{ pgsql_version }}"
pgsql_config_dir: "/etc/postgresql/{{ pgsql_version }}/main"
pgsql_new_data_dir: /tmp/postgresql

users_hba_method: md5
replication_hba_method: md5

pgsql_users:

- name: linhitmo
  password: 123456
  encrypted: true
  role_attr_flags: SUPERUSER
  port: 5432

pgsql_databases:

- name: linhitmoDB
  encoding:
  lc_collate:
  lc_ctype:
  conn_limit:
  port: 5432
  roles: linhitmo
  privs: ALL

master_address:

- 192.168.56.201
  replica_address:
- 192.168.56.202

## Dependencies

## Example Playbook

**Pay Attention!**
You need to provide accessible IP addresses to `ansible_ssh_host` variable in inventory file:

    [master]
    server1 ansible_ssh_host=192.168.56.201

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: Setup Master
      hosts: master
      become: true

      tasks:
        - name: "Include pgsql for master role"
          include_role:
            name: pgsql
          vars:
            postgresql_role: master

    - name: Setup Replica
      hosts: replica
      become: true

      tasks:
        - name: "Include pgsql for replica role"
          include_role:
            name: pgsql
          vars:
            postgresql_role: replica

## License

BSD

## Author Information

Duong Thi Hue Linh, ITMO University
