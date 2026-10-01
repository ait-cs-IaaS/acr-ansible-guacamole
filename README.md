# acr-ansible-guacamole
Ansible role to install and configure Apache Guacamole on the AIT Cyberrange.

Heavily inspired from https://github.com/mrlesmithjr/ansible-guacamole, but specifically targets Ubuntu >= 24.04.

Before porting to another installation target, please check the comments in `defaults/main.yml`!

## LDAP Authentication

LDAP authentication is optional and can be enabled by setting `guacamole_ldap_enable: true`. When enabled, the role downloads and installs the official `guacamole-auth-ldap` extension and configures the LDAP properties in `guacamole.properties`.

### Role Variables

| Variable | Default | Description |
|---|---|---|
| `guacamole_ldap_enable` | `false` | Enable or disable LDAP authentication |
| `guacamole_ldap_hostname` | `localhost` | Hostname or IP of LDAP directory server |
| `guacamole_ldap_port` | `389` | Port of LDAP server (e.g. `389` or `636` for LDAPS) |
| `guacamole_ldap_encryption_method` | `none` | Encryption method: `none`, `ssl`, or `starttls` |
| `guacamole_ldap_ssl_protocol` | `TLSv1.3` | SSL/TLS protocol version (`TLSv1.2`, `TLSv1.3`, etc.) |
| `guacamole_ldap_user_base_dn` | `""` | Base DN containing user accounts (e.g. `ou=people,dc=example,dc=com`) |
| `guacamole_ldap_username_attribute` | `uid` | Attribute containing username (e.g. `uid` or `sAMAccountName`) |
| `guacamole_ldap_search_bind_dn` | `""` | DN used to bind and search for users |
| `guacamole_ldap_search_bind_password` | `""` | Password for `guacamole_ldap_search_bind_dn` |
| `guacamole_ldap_config_base_dn` | `""` | Base DN for Guacamole connection configurations (optional) |
| `guacamole_ldap_group_base_dn` | `""` | Base DN for user groups (optional) |
| `guacamole_ldap_group_name_attribute` | `cn` | Attribute containing group name (optional) |
| `guacamole_ldap_user_search_filter` | `""` | LDAP filter to query users (e.g. `(objectClass=inetOrgPerson)`) |
| `guacamole_ldap_group_search_filter` | `""` | LDAP filter to query groups (optional) |
| `guacamole_ldap_user_attributes` | `""` | Comma-separated LDAP attributes to retrieve as parameter tokens |
| `guacamole_ldap_member_attribute` | `member` | Attribute containing members within group objects |
| `guacamole_ldap_member_attribute_type` | `dn` | Group member attribute type (`dn` or `uid`) |
| `guacamole_ldap_dereference_aliases` | `never` | Dereference aliases: `never`, `searching`, `finding`, `always` |
| `guacamole_ldap_follow_referrals` | `false` | Whether to follow LDAP referrals |
| `guacamole_ldap_max_referral_hops` | `5` | Maximum number of referrals to follow |
| `guacamole_ldap_operation_timeout` | `30` | Timeout in seconds for LDAP operations |
| `guacamole_ldap_max_search_results` | `1000` | Maximum number of results for an LDAP query |

### Example Playbook

```yaml
- hosts: guacamole
  roles:
    - role: acr-ansible-guacamole
      vars:
        guacamole_ldap_enable: true
        guacamole_ldap_hostname: ldap.example.com
        guacamole_ldap_port: 636
        guacamole_ldap_encryption_method: ssl
        guacamole_ldap_user_base_dn: ou=users,dc=example,dc=com
        guacamole_ldap_username_attribute: uid
        guacamole_ldap_search_bind_dn: cn=guac-bind,ou=services,dc=example,dc=com
        guacamole_ldap_search_bind_password: "{{ vault_ldap_bind_password }}"
        guacamole_ldap_group_base_dn: ou=groups,dc=example,dc=com
```
