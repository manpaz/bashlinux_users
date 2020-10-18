bashlinux_users
===============

This role creates a group and the users belonging to the same group, when the list of users is provided.

Role Variables
--------------

The settable variables for this role are:
```yaml
group_name: ""          # Group Name. If not provided playbook nothing will be done and a message saying so will be displayed
                        # (mandatory)
group_id: ""            # Group ID. If not provided, next gid available on system will be used
                        # Desired if the same "gid" is needed across systems
                        # (default empty)
group_sudo: false       # Flag granting sudo privileges to members of this group
                        # (default false)
group_users: ""         # Name of the variable holding the list of users. If not provided, no users will be created
                        # (default empty)
```

Ideally, one might need to either, enable the `group_sudo` flag, pass the list of users through `group_users`, or both, otherwise the `group` module should suffice the group creation.


Example Playbook
----------------

Create a group and enable sudo for its members

```yaml
---
- hosts: servers
  vars:
    group_name: privileged_group_name
    group_sudo: true
  roles:
    - bashlinux_users
```

Create a non-privileged group, with a consistent `gid` across all servers and provide a list of users, with consistent gid. The public ssh key will be set if provided.
```yaml
---
- hosts: servers
  vars:
    group_name: nonprivileged_group_name
    group_id: "{{ nonprivileged_group_name.gid }}"
    group_users: "{{ nonprivileged_group_name.users }}"
  roles:
    - bashlinux_users
```

where the list of users is the form of:
```yaml
---
nonprivileged_group_name:
  gid: 3001
  users:
    - name: user_alpha
      uid: 2001
      ssh_key: "ssh-rsa AAABBBCCCA1....== user_alpha@example.com"
    - name: user_beta
      uid: 2002
      ssh_key: "ssh-rsa AAABBBCCCB2....== user_beta@example.com"

```

License
-------

LGPL-3.0-or-later
