harbor-server
=============

An Ansible(tm) role Installs Harbor(tm) from VMware(tm) as the dependancies from Docker(tm).

* http://vmware.github.io/harbor/

Work in progress. Currently deploys from a local file, tested on CentOS 7

Requirements
------------

openssl needed for self signed certificates

Role Variables
--------------

Some defaults:

Installation sources / parameters:

```
harbor_install_tmp: /tmp/harbor
harbor_install_dir: /tmp/harbor_install
harbor_install_download: https://storage.googleapis.com/harbor-releases/release-1.4.0/harbor-offline-installer-v1.4.0.tgz
harbor_install_tgz: harbor-installer.tgz
harbor_install_upload_localcopy_of_installer:  #if set it installs from this address
harbor_install_skip_docker_compose: 'False' # speed up testing / or Docker + Docker Compose already installed
harbor_ssl_self_days:     # valid days for self signed certificate
harbor_ssl_self_subject:  # dn for  self signed certificate
harbor_ssl_cert:          # certificate location
harbor_ssl_cert_key:      # private key location 

    
```
ends up in  harbor.cfg
```
    harbor_hostname: localhost  # should not be localhost... registry might not be accessible from outside
    harbor_ui_url_protocol: https
    harbor_db_password: root123
    harbor_customize_crt: on
    harbor_secretkey_path: /data
    harbor_admin_password: HarborAdminPassword
    harbor_auth_mode: db_auth
    harbor_self_registration: off
    harbor_project_creation_restriction: adminonly
    harbor_verify_remote_cert: on
```

Dependencies
------------

None?

Example Playbook
----------------

Install using a locally hosted copy of the installation tar:

    - hosts: harbor-servers
      name: install VMWare Harbor registry
      roles:
        - harbor-server
      vars:
        - harbor_install_upload_localcopy_of_installer: /Your/local/path/harbor-offline-installer-v1.4.0.tgz
        - harbor_install_https_self_signed: True # need to add check
        - harbor_ui_url_protocol: https
        - harbor_ssl_cert: '{{ harbor_secretkey_path }}/cert/server.crt'
        - harbor_ssl_cert_key: '{{ harbor_secretkey_path }}/cert/server.key'

Example Inventory
-----------------

[harbor-servers]
server1  ansible_become=true ansible_become_method=sudo ansible_port=22 ansible_host=x.x.x.x ansible_user=user ansible_become_pass=user-pass

Example Executing Playbook
--------------------------

    ansible-playbook site.yaml -i inventory -v

License
-------

BSD

Author Information
------------------

This is a modification of https://github.com/mkgin/ansible-harbor-server
