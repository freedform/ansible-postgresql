# postgresql

Role postgresql automates installation, configuration, and replication setup of PostgreSQL

## Table of contents

- [Requirements](#requirements)
- [Default Variables](#default-variables)
  - [postgresql_actions](#postgresql_actions)
  - [postgresql_config](#postgresql_config)
  - [postgresql_config_base_dir](#postgresql_config_base_dir)
  - [postgresql_data_backup](#postgresql_data_backup)
  - [postgresql_data_base_dir](#postgresql_data_base_dir)
  - [postgresql_file_group](#postgresql_file_group)
  - [postgresql_file_owner](#postgresql_file_owner)
  - [postgresql_major_version](#postgresql_major_version)
  - [postgresql_peer_ip](#postgresql_peer_ip)
  - [postgresql_replication_password](#postgresql_replication_password)
  - [postgresql_replication_slot](#postgresql_replication_slot)
  - [postgresql_replication_username](#postgresql_replication_username)
  - [postgresql_role](#postgresql_role)
  - [postgresql_state](#postgresql_state)
  - [postgresql_version](#postgresql_version)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Requirements

- Minimum Ansible version: `2.20`
- For the `replication` action on a `postgresql_role: primary` node: `ansible_python_interpreter`
  must be executable by the `postgres` OS user and have `psycopg2` installed. These tasks run as
  `postgres` via `become`, so an interpreter reachable only by the connection user (e.g. a venv
  under that user's home directory with restrictive permissions) will not work.

## Default Variables

### postgresql_actions

List of actions the role does, accepts one or more actions.
Use comma without spaces as a delimiter for multiple actions.

**_Required:_** `true`<br />
**_Type:_** String<br />

#### Example usage

```YAML
  postgresql_actions: install
  postgresql_actions: install,upload_config
```

### postgresql_config

PostgreSQL configuration mapping. Supports main (postgresql.conf directives as key-value pairs)
and hba (pg_hba.conf entries as a list of strings).

**_Required:_** `true`, only when action is upload_config<br />
**_Type:_** Dict<br />

#### Example usage

```YAML
postgresql_config:
  main:
    listen_addresses: "'*'"
    max_connections: 100
  hba:
    - local   all   all   trust
    - host    replication   replicator   192.168.1.0/24   md5
```

### postgresql_config_base_dir

Base directory for PostgreSQL configuration; combined with postgresql_major_version to form postgresql_config_dir

**_Type:_** String<br />

#### Default value

```YAML
postgresql_config_base_dir: /etc/postgresql
```

### postgresql_data_backup

Whether to back up the data directory before pg_basebackup during replica setup

**_Type:_** Boolean<br />

#### Default value

```YAML
postgresql_data_backup: true
```

### postgresql_data_base_dir

Base directory for PostgreSQL data; combined with postgresql_major_version to form postgresql_data_dir

**_Type:_** String<br />

#### Default value

```YAML
postgresql_data_base_dir: /var/lib/postgresql
```

### postgresql_file_group

Group of PostgreSQL configuration files

**_Type:_** String<br />

#### Default value

```YAML
postgresql_file_group: postgres
```

### postgresql_file_owner

Owner of PostgreSQL configuration files

**_Type:_** String<br />

#### Default value

```YAML
postgresql_file_owner: postgres
```

### postgresql_major_version

Major PostgreSQL version. Combined with postgresql_config_base_dir and postgresql_data_base_dir
to derive postgresql_package, postgresql_config_dir, and postgresql_data_dir in vars/main.yml.

**_Type:_** Integer<br />

#### Default value

```YAML
postgresql_major_version: 16
```

### postgresql_peer_ip

IP address of the peer node (primary IP on replica, replica IP on primary)

**_Required:_** `true`, only when action is replication<br />
**_Type:_** String<br />

### postgresql_replication_password

Password for the replication user

**_Required:_** `true`, only when action is replication<br />
**_Type:_** String<br />

### postgresql_replication_slot

Replication slot name to create on primary and use on replica

**_Required:_** `true`, only when action is replication<br />
**_Type:_** String<br />

### postgresql_replication_username

Username for the replication user

**_Required:_** `true`, only when action is replication<br />
**_Type:_** String<br />

### postgresql_role

Replication role for this node

**_Required:_** `true`, only when action is replication or promote<br />
**_Type:_** String<br />

#### Example usage

```YAML
  postgresql_role: primary
  postgresql_role: replica
```

### postgresql_state

Target state for the PostgreSQL daemon

**_Required:_** `true`, only when action is state_control<br />
**_Type:_** String<br />

#### Example usage

```YAML
  postgresql_state: restarted
```

### postgresql_version

Exact apt package version string to pin the install to, e.g. 18.6-1.pgdg24.04+2. Must match the
version format published in the PGDG repo for the target distribution release, not the Ubuntu/Debian
archive format - the two differ. Left empty by default, which installs the latest version available
for postgresql_major_version in the enabled PGDG repo instead of pinning to a specific one.

**_Type:_** String<br />

#### Default value

```YAML
postgresql_version: ''
```

## Dependencies

None.

## License

MIT

## Author

freedform
