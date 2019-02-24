Nginx Loadbalancer
==================

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

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Inventory
-----------------


    [all]
    lb ansible_host=192.168.1.1 ansible_user=vagrant ansible_ssh_private_key_file=.vagrant/machines/lb/virtualbox/private_key ansible_ssh_common_args="-o 'StrictHostKeyChecking no'"
    web1 ansible_host=192.168.2.1 ansible_user=vagrant ansible_ssh_private_key_file=.vagrant/machines/web1/virtualbox/private_key ansible_ssh_common_args="-o 'StrictHostKeyChecking no'"
    web2 ansible_host=192.168.2.2 ansible_user=vagrant ansible_ssh_private_key_file=.vagrant/machines/web2/virtualbox/private_key ansible_ssh_common_args="-o 'StrictHostKeyChecking no'"
    
    [lb]
    lb
    
    [web]
    web1
    web2

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: Provision boxes
      hosts: all
      become: true
      roles:
        - { role: all, tags: [ 'common', 'all' ] }
    
    - name: Set up the web server
      hosts: web
      become: true
      roles: 
        - { role: nginx, tags: [ 'nginx', 'web' ] }
    
    - name: Setup load balancer
      hosts:
        - lb
      become: true
      roles:
        - { role: loadbalancer, tags: ['lb', 'nginx', 'web']}

License
-------

MIT

Author Information
------------------

Michelangelo van Dam (michelangelo+github@in2it.be)
