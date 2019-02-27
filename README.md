Nginx Loadbalancer
==================

[![Build Status](https://travis-ci.org/in2it-ansible/nginx_loadbalancer.svg?branch=master)](https://travis-ci.org/in2it-ansible/nginx_loadbalancer)

This role sets up a simple load balancing reverse proxy using [Nginx](https://www.nginx.org).

Requirements
------------

This role is used to distribute load over running web services, these web services should be defined separately.

Role Variables
--------------

Here are the variables defined in `vars/main.yml` overriding `defaults/main.yml`:

- **nginx_timezone:** The timezone where the system runs (default: UTC)
- **nginx_apt_key:** The official GPG public key from Nginx (default: https://nginx.org/keys/nginx_signing.key)
- **nginx_apt_keyid:** The ID of the official GPG public key from Nginx (default: ABF5BD827BD9BF62)
- **nginx_apt_list:** The official Nginx repository for Debian (default: "deb http://nginx.org/packages/debian {{ ansible_lsb.codename }} nginx")
- **server_hostname:** The hostname Nginx listens to (default: lb.example.com)
- **package:** The package to install with APT (default: nginx)

The following variables are used in `templates/loadbalancer.conf`:

- **ansible_host:** The hostname or IP address of the hosts used within the loadbalancer (see Example Inventory)

Dependencies
------------

No dependencies

Example Inventory
-----------------


    [all]
    lb ansible_host=192.168.1.1
    web1 ansible_host=192.168.2.1
    web2 ansible_host=192.168.2.2
    
    [lb]
    lb
    
    [web]
    web1
    web2

Example Playbook
----------------

    - name: Provision boxes
      hosts: all
      become: true
      roles:
        - { role: all, tags: [ 'common', 'all' ] }
    
    - name: Set up the web server
      hosts: web
      become: true
      roles: 
        - { role: dragonbe.nginx_fcgi, tags: [ 'nginx', 'web', 'fcgi' ] }
    
    - name: Setup load balancer
      hosts:
        - lb
      become: true
      roles:
        - { role: dragonbe.nginx_loadbalancer, tags: ['lb', 'nginx', 'web']}

License
-------

MIT

Author Information
------------------

Michelangelo van Dam (michelangelo+github@in2it.be)
