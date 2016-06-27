OULibraries.nginx
=========

Nginx webserver for OU Libraries. Uses official nginx stable repository.
Currently, I've only added the sauce for self-signed certs and wildcard hostnames.
Also, it only proxies the first upstream defined per site.
SSL over port 443 is hardcoded into the upstreams.
Will add more stuff later.

Requirements
------------

CentOS 7x


Role Variables
--------------

You'll need to define hostnames, backend ip, and an optional robots.txt overlay, eg.
```
nginx_star_sites:
 - name: example.com
   upstream:
     - name: example-dev
       ip:
         - 192.168.1.10
         - 192.168.1.11
         - 192.168.1.12
   robots: disallow
 - name: dspace.example.com
   upstream:
     - name: dspace-dev
       ip:
         - 192.168.1.13
   robots: dspace
```

Any name you enter will be configured with `*.` in front of it.

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
