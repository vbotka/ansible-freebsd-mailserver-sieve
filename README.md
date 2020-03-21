# freebsd_mailserver_sieve

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mailserver-sieve.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mailserver-sieve)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_mailserver_sieve/) FreeBSD. Install and configure dovecot-pigeonhole (Sieve RFC5228).

Please feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-mailserver-sieve/issues).

## Dependencies

- [vbotka.freebsd_mailserver](https://galaxy.ansible.com/vbotka/freebsd_mailserver/) Install and configure Postfix and Dovecot.
- [vbotka.ansible_lib](https://galaxy.ansible.com/vbotka/ansible_lib) Library of Ansible tasks.


## Variables

Review the defaults and examples in vars.


## Workflow

1) Change shell to /bin/sh

```
shell> ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role

```
shell> ansible-galaxy install vbotka.freebsd_mailserver_sieve
```

3) Fit variables

```
shell> editor vbotka.freebsd_mailserver_sieve/vars/main.yml
```

4) Create playbook and inventory

```
shell> cat freebsd-mailserver-sieve.yml

- hosts: mailserver
  roles:
    - vbotka.freebsd_mailserver
    - vbotka.freebsd_mailserver_sieve
```

```
shell> cat hosts
[mailserver]
<mailserver-ip-or-fqdn>
[mailserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=yes
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python3.7
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure Sieve.

```
shell> ansible-playbook freebsd-mailserver-sieve.yml
```

6) Consider to test the mailserver with http://mxtoolbox.com/


## User configuration

1) Create .forward in user home directory

```
| "/usr/local/libexec/dovecot/dovecot-lda"
```

2) Create filter /home/user/sieve/managesieve.sieve

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


## References

- [Pigeonhole Sieve Configuration](http://wiki2.dovecot.org/Pigeonhole/Sieve/Configuration)
- [ManageSieve Configuration](http://wiki2.dovecot.org/Pigeonhole/ManageSieve/Configuration)
- [Pigeonhole Sieve examples](http://wiki2.dovecot.org/Pigeonhole/Sieve/Examples)


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.link)
