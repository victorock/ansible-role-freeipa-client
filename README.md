ipaclient role
==============

Description
-----------

This role allows to join hosts as clients to an IPA domain. This can be done in differnt ways using auto-discovery of the servers, domain and other settings or by specifying them.

Usage
-----

Example inventory file with fixed principal using auto-discovery with DNS records:

    [ipaclients]
    ipaclient1.example.com
    ipaclient2.example.com

    [ipaclients:vars]
    ipaadmin_principal=admin

Example playbook to setup the IPA client(s) using principal from inventory file and password from an [Ansible Vault](http://docs.ansible.com/ansible/latest/playbooks_vault.html) file:

    - name: Playbook to configure IPA clients with username/password
      hosts: ipaclients
      become: true
      vars_files:
      - playbook_sensitive_data.yml

      tasks:
        - include_role:
            name: ipaclient
            tasks_from: run
          vars:
            state: present

Example playbook to unconfigure the IPA client(s) using principal and password from inventory file:

    - name: Playbook to unconfigure IPA clients
      hosts: ipaclients
      become: true

      tasks:
        - include_role:
            name: ipaclient
            tasks_from: run
          vars:
            state: basent

Example inventory file with fixed servers, principal, password and domain:

    [ipaclients]
    ipaclient1.example.com
    ipaclient2.example.com

    [ipaservers]
    ipaserver.example.com

    [ipaclients:vars]
    ipaclient_domain=example.com
    ipaadmin_principal=admin
    ipaadmin_password=MySecretPassword123

Example playbook to setup the IPA client(s) using principal and password from inventory file:

    - name: Playbook to configure IPA clients with username/password
      hosts: ipaclients
      become: true

      tasks:
        - include_role:
            name: ipaclient
            tasks_from: run
          vars:
            state: present

Variables
---------

**ipaservers** - Group of IPA server hostnames.
 (list of strings, optional)

**ipaclients** - Group of IPA client hostnames.
 (list of strings)

**ipaadmin_keytab** - The path to the admin keytab used for alternative authentication.
 (string, optional)

**ipaadmin_principal** - The authorized kerberos principal used to join the IPA realm.
 (string, optional)

**ipaadmin_password** - The password for the kerberos principal.
 (string, optional)

**ipaclient_domain** - The primary DNS domain of an existing IPA deployment.
 (string, optional)

**ipaclient_realm** - The Kerberos realm of an existing IPA deployment.
 (string, optional)

**ipaclient_keytab** - The path to a backed-up host keytab from previous enrollment.
 (string, optional)

**ipaclient_force_join** - Set force_join to yes to join the host even if it is already enrolled.
 (bool, optional)

**ipaclient_use_otp** - Enforce the generation of a one time password to configure new and existing hosts. The enforcement on an existing host is not done if there is a working krb5.keytab on the host. If the generation of an otp is enforced for an existing host entry, then the host gets diabled and the containing keytab gets removed.
 (bool, optional)

**ipaclient_allow_repair** - Allow repair of already joined hosts. Contrary to ipaclient_force_join the host entry will not be changed on the server.
 (bool, optional)

**ipaclient_kinit_attempts** - Repeat the request for host Kerberos ticket X times if it fails.
 (int, optional)

**ipaclient_ntp** - Set to no to not configure and enable NTP
 (bool, optional)

**ipaclient_mkhomedir** - Set to yes to configure PAM to create a users home directory if it does not exist.
 (string, optional)

Requirements
------------

freeipa-client v4.4 or later

Authors
-------

Florence Blanc-Renaud

Thomas Woerner

Victor da Costa
