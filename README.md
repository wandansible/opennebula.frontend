Ansible role: OpenNebula Frontend
=================================

Install and configure OpenNebula.
Deploy an OpenNebula frontend server that functions as a
controller for an OpenNebula cluster and serves the sunstone
web GUI.

Role Variables
--------------

```
ENTRY POINT: main - Install and configure OpenNebula

        Deploy an OpenNebula frontend server that functions as a
        controller for an OpenNebula cluster and serves the sunstone
        web GUI.

OPTIONS (= is mandatory):

= ldap_auth_base
        Base DN for LDAP search

        type: str

= ldap_auth_host
        Hostname of LDAP server for OpenNebula LDAP authentication

        type: str

= ldap_auth_port
        Port for LDAP server

        type: int

- one_addon_lvm_version
        Version of the LVM driver addon to install, for available
        versions see: https://github.com/wandnz/addon-lvm/releases
        [Default: 1.0.0]
        type: str

= one_admin_password
        Password for the oneadmin user for authentication with
        OpenNebula

        type: str

- one_apt_key_fingerprint
        Fingerprint for the OpenNebula apt key
        [Default: (null)]
        type: str

- one_config
        Configuration for oned
        [Default: (null)]
        type: dict

        OPTIONS:

        = sections
            Configuration for whole sections

            elements: dict
            type: list

            OPTIONS:

            - match
                Search regex for matching a specific section when
                there a multiple sections with the same name
                [Default: (null)]
                type: str

            = name
                Name of the section

                type: str

            = value
                Contents for the whole section

                type: str

        = vars
            Configuration for individual variables that aren't in a
            section

            type: dict

- one_datastore_mounts
        Disk mounts for individual datastores
        [Default: (null)]
        elements: dict
        type: list

        OPTIONS:

        = disk_uuid
            UUID of the disk being mounted

            type: str

        = id
            ID of the datastore to mount to

            type: int

- one_datastores
        Configuration for datastores
        [Default: (null)]
        elements: dict
        type: list

        OPTIONS:

        = config
            Configuration for the datastore

            type: dict

        - id
            ID of the datastore (required for renaming)
            [Default: (null)]
            type: int

        = name
            Name for the datastore

            type: str

- one_datastores_disk_uuid
        UUID of the disk to mount to the datastores root directory
        [Default: (null)]
        type: str

= one_db_password
        Password for connecting to the OpenNebula database

        type: str

- one_debug_level
        Log level
        0 = ERROR
        1 = WARNING
        2 = INFO
        3 = DEBUG
        (Choices: 0, 1, 2, 3)[Default: 1]
        type: int

= one_frontend
        Hostname of the frontend server

        type: str

= one_frontend_key
        Contents of the frontend SSH host key

        type: str

- one_install_version
        Version of OpenNebula to install
        [Default: 6.4.0]
        type: str

- one_ldap_tls_options
        TLS options for LDAP connections
        [Default: (null)]
        type: dict

- one_pyone_version
        Version of pyone to install
        [Default: 6.4.0]
        type: str

- one_ssh_key_type
        The oneadmin ssh key type
        [Default: ed25519]
        type: str

= one_ssh_privkey
        Contents of the oneadmin ssh private key

        type: str

= one_ssh_pubkey
        Contents of the oneadmin ssh public key

        type: str

- one_sunstone_config
        Configuration for sunstone
        [Default: (null)]
        type: dict

- one_sunstone_view_features
        Configuration for sunstone view features
        [Default: (null)]
        elements: dict
        type: list

        OPTIONS:

        = key
            Name of the feature

            type: str

        - type
            The environment type
            (Choices: mixed, kvm, vcenter)[Default: mixed]
            type: str

        = value
            If true, enable the feature

            type: bool

        = view
            The view to configure

            type: str

- one_version
        Version of the OpenNebula repository to track
        [Default: 6.4]
        type: str
```

Installation
------------

This role can either be installed manually with the ansible-galaxy CLI tool:

    ansible-galaxy install git+https://github.com/wandansible/opennebula.frontend,main,wandansible.opennebula.frontend
     
Or, by adding the following to `requirements.yml`:

    - name: wandansible.opennebula.frontend
      src: https://github.com/wandansible/opennebula.frontend

Roles listed in `requirements.yml` can be installed with the following ansible-galaxy command:

    ansible-galaxy install -r requirements.yml

Example Playbook
----------------

    - hosts: opennebula_frontend_servers
      roles:
         - role: opennebula.frontend
           become: true
           vars:
             one_version: "5.12"
             one_pyone_version: "5.12.0"

             one_frontend: "cloud.example.org"
             one_api_username: ansible
             one_api_password: CHANGEME
             one_db_password: CHANGEME
             one_admin_password: CHANGEME
             one_frontend_key: "ssh-ed25519 AAAA..."
             one_ssh_pubkey: "ssh-ed25519 AAAA..."
             one_ssh_privkey: |
               -----BEGIN OPENSSH PRIVATE KEY-----
               XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
               -----END OPENSSH PRIVATE KEY-----

             one_datastores_disk_uuid: aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
             one_datastore_mounts:
               - id: 0
                 disk_uuid: vvvvvvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz

             ldap_auth_host: "ldap.example.org"
             ldap_auth_port: "636"
             ldap_auth_base: "dc=example,dc=org"
