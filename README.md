OULibraries.nginx
=========

Nginx webserver for OU Libraries. Uses official nginx stable repository.
It only proxies the first upstream defined per site.

Installs LetsEncrypt Certbot, but doesn't actually run letsencrypt for you.  It should automatically renew any certificates created through Certbot, however. Assuming you have DNS correctly configured alread, you should be able to run the following after applying this role:

```
sudo certbot certonly --webroot -w /srv/certbot -d site.example.com --email email@example.com --agree-tos

```

and have appropriately certificates created. You'll need to substitute in appropriate directories, hostnames, and email addresses. This role is currently too dumb to handle the changeover of self-signed certs to real certs in the nginx config, so you're on your own at the moment.


Will add more stuff later.

Requirements
------------

CentOS 7x


Role Variables
--------------

You'll need to define hostnames, backend servers, and an optional robots.txt overlay, eg.
```
nginx_star_sites:
 - name: example.com
   upstreams:
     - name: example-dev
       servers:
         - 192.168.1.10:443
         - 192.168.1.11:443
         - 192.168.1.12:443
   robots: disallow
 - name: dspace.example.com
   upstreams:
     - name: dspace-dev
       servers:
         - 192.168.1.13:8443
   robots: dspace
nginx_literal_sites:
 - name: 1.example.com
   upstreams:
     - name: 1-example-dev
       servers:
         - 192.168.1.13:9443
         - 192.168.1.14:9445
         - 192.168.1.15:9955
   robots: disallow
nginx_stub_sites:
 - name: 1.example.com
   robots: disallow
```

All backends must use SSL regardless of port.

Any name you enter for a star site will be configured with `*.` in front of it.

See defaults/main.yml for the rest

Dependencies
------------


Example Playbook
----------------
An example vagrant playbook.

```
- hosts: nginx.vagrant.local
  sudo: yes
  vars_files:
    - my-vars.yml
  pre_tasks:
    - copy:
        src: /vagrant/dhparam.pem
        dest: "{{ nginx_cert_path }}/dhparam.pem"
  roles:
    - OULibraries.nginx
```

This example has a pretask that copies over static DH parameters to speed provisioning.  This is fine for a test environment, but not suitable for production.

License
-------

TBD

Author Information
------------------

Jason Sherman
