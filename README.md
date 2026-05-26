# postgresql

Role postgresql automates installation, configuration, and replication setup of PostgreSQL

## Table of contents

- [Requirements](#requirements)
- [Default Variables](#default-variables)
  - [postgresql_actions](#postgresql_actions)
  - [postgresql_ansible_temp_password](#postgresql_ansible_temp_password)
  - [postgresql_config](#postgresql_config)
  - [postgresql_config_dir](#postgresql_config_dir)
  - [postgresql_data_backup](#postgresql_data_backup)
  - [postgresql_data_dir](#postgresql_data_dir)
  - [postgresql_file_group](#postgresql_file_group)
  - [postgresql_file_owner](#postgresql_file_owner)
  - [postgresql_major_version](#postgresql_major_version)
  - [postgresql_package](#postgresql_package)
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

### postgresql_ansible_temp_password

Temporary password for the ansible superuser role created on the primary during replication setup.
This role is dropped immediately after setup completes.

**_Required:_** `true`, only when action is replication and postgresql_role is primary<br />
**_Type:_** String<br />

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

### postgresql_config_dir

PostgreSQL configuration directory, derived from postgresql_major_version

**_Type:_** String<br />

#### Default value

```YAML
postgresql_config_dir: /etc/postgresql/{{ postgresql_major_version }}/main
```

### postgresql_data_backup

Whether to back up the data directory before pg_basebackup during replica setup

**_Type:_** Boolean<br />

#### Default value

```YAML
postgresql_data_backup: true
```

### postgresql_data_dir

PostgreSQL data directory, derived from postgresql_major_version

**_Type:_** String<br />

#### Default value

```YAML
postgresql_data_dir: /var/lib/postgresql/{{ postgresql_major_version }}/main
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

Major PostgreSQL version. Drives the package name, config dir, and data dir.
Update postgresql_version to match when changing this.

**_Type:_** Integer<br />

#### Default value

```YAML
postgresql_major_version: 16
```

### postgresql_package

PostgreSQL apt package name, derived from postgresql_major_version

**_Type:_** String<br />

#### Default value

```YAML
postgresql_package: postgresql-{{ postgresql_major_version }}
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

Full apt package version string for installation

**_Type:_** String<br />

#### Default value

```YAML
postgresql_version: 16.11-0ubuntu0.24.04.1
```

## Dependencies

None.

## License

MIT

## Author

freedform
