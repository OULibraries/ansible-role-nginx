OULibraries.nginx
=========

Nginx webserver for OU Libraries. Uses official nginx stable repository.
It only proxies the first upstream defined per site.

Can deploy letsencrypt certificates for literal_sites that have the element cert_type defined and set to "letsencrypt".  Currently anything else (leaving the cert_type out, or setting it to something other than "letsencrypt" defaults to the old behavior of creating a self-signed certificate.

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
   cert_type: letsencrypt # this is optional
   redirect_locations: # this is optional
     - src: /source
       dest: /destination
       code: 302
   upstreams:
     - name: 1-example-dev
       servers:
         - 192.168.1.13:9443
         - 192.168.1.14:9445
         - 192.168.1.15:9955
       locations:
         - /
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
  become: true
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

[MIT](https://github.com/OULibraries/ansible-role-nginx/blob/master/LICENSE)

Author Information
------------------

Jason Sherman
