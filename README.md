OULibraries.nginx
=========

Nginx webserver for OU Libraries. Uses official nginx stable repository.
Currently, I've only added the sauce for self-signed certs.
Also, it only proxies the first upstream defined per site.
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
```

All backends must use SSL regardless of port.

Any name you enter for a star site will be configured with `*.` in front of it.

See defaults/main.yml for the rest

Dependencies
------------


Example Playbook
----------------


License
-------

TBD

Author Information
------------------

Jason Sherman
