freebsd-mailserver-sieve
========================

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mailserver-sieve.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mailserver-sieve)

Ansible role. Install and configure dovecot-pigeonhole (Sieve RFC5228).

https://galaxy.ansible.com/vbotka/freebsd-mailserver-sieve/


Requirements
------------

- [vbotka.freebsd-mailserver](https://galaxy.ansible.com/vbotka/freebsd-mailserver/)


Variables
---------

TBD. Check the defaults.


Workflow
--------

1) Change shell to /bin/sh.

```
# ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
# ansible-galaxy install vbotka.freebsd-mailserver-sieve
```

3) Fit variables.

```
# editor vbotka.freebsd-mailserver-sieve/vars/main.yml
```

4) Create playbook and inventory.

```
# cat freebsd-mailserver-sieve.yml

- hosts: mailserver
  roles:
    - vbotka.freebsd-mailserver-sieve
```

```
# cat hosts
[mailserver]
<mailserver-ip-or-fqdn>
[mailserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=yes
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python2.7
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure Sieve.

```
# ansible-playbook freebsd-mailserver-sieve.yml
```

6) Consider to test the mailserver with http://mxtoolbox.com/


User configuration
------------------

1) Create .forward in user home directory

```
| "/usr/local/libexec/dovecot/dovecot-lda"
```

2) # Create filter /home/user/sieve/managesieve.sieve

```
require ["fileinto"];
# rule:[spam-01]
if allof (header :contains "subject" "*****SPAM*****")
{
	fileinto "Spam";
	stop;
}
# rule:[spam-02]
if header :contains "X-Spam-Flag" "YES" {
  fileinto "Spam";
	   }
```

3) Create directory for spam

```
~/Maildir/Spam
```

If this directory shall be visible in the Roundcube webmail create the Spam directory in Roundcube Settings->Folders


References
----------

[Pigeonhole Sieve Configuration](http://wiki2.dovecot.org/Pigeonhole/Sieve/Configuration)

[ManageSieve Configuration](http://wiki2.dovecot.org/Pigeonhole/ManageSieve/Configuration)

[Pigeonhole Sieve examples](http://wiki2.dovecot.org/Pigeonhole/Sieve/Examples)


License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


Author Information
------------------

[Vladimir Botka](https://botka.link)
