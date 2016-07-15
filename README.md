# Ansible letsencrypt nginx webroot role


This is an [Ansible](http://www.ansible.com) role for Ubuntu 14.04 (did not test on other distros) which:

 * downloads letsencrypt from git repo
 * installs config with webroot authenticator
 * runs letsencrypt to get the certificates if it can find none 
 * creates cron job that checks certificates expire dates, rotates them when it is required and reloads nginx 

## Installation

Using `ansible-galaxy`:

```shell
$ ansible-galaxy install biomancer.letsencrypt_webroot
```

There must be specified acme-challenge location in nginx config for all domains from `letsencrypt_domains` var:
```
location /.well-known/acme-challenge {
  allow all;
  default_type  "text/plain";
  root          /opt/letsencrypt-webroot;
}
```

## Known issues

Modifying `letsencrypt_domains` list when `/etc/letsencrypt/live/example.com` certs dir already exists will rename it from `/etc/letsencrypt/live/example.com/` to `/etc/letsencrypt/live/example.com-0001`

## Usage

This is an example playbook:

```yaml
---

- hosts: all
  sudo: yes
  roles:
    - biomancer.letsencrypt_webroot
  vars:
    letsencrypt_email: example@example.com
    # TODO modifying this list will change certs folder from /etc/letsencrypt/live/example.com/ to /etc/letsencrypt/live/example.com-0001
    letsencrypt_domains:
      - example.com
      - www.example.com
```

## License
The MIT license.
