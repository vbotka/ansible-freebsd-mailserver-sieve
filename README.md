# freebsd_mailserver_sieve

[![quality](https://img.shields.io/ansible/quality/27910)](https://galaxy.ansible.com/vbotka/freebsd_mailserver_sieve)
[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mailserver-sieve.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mailserver-sieve)
[![GitHub tag](https://img.shields.io/github/v/tag/vbotka/ansible-freebsd-mailserver-sieve)](https://github.com/vbotka/ansible-freebsd-mailserver-sieve/tags)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_mailserver_sieve/) FreeBSD. Install and configure dovecot-pigeonhole (Sieve RFC5228).

Feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-mailserver-sieve/issues).

[Contributions are welcome](https://github.com/firstcontributions/first-contributions).


## Requirements and dependencies

### Roles

The dependencies are not listed in the meta file. Install them manually.

- [vbotka.freebsd_mailserver](https://galaxy.ansible.com/vbotka/freebsd_mailserver/) Install and configure Postfix and Dovecot.
- [vbotka.ansible_lib](https://galaxy.ansible.com/vbotka/ansible_lib) Library of Ansible tasks.


### Collections

- community.general


## Variables

Review the defaults and examples in vars.


## Workflow

1) Change shell to /bin/sh if necessary

```bash
shell> ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install roles

```bash
shell> ansible-galaxy role install vbotka.freebsd_mailserver
shell> ansible-galaxy role install vbotka.freebsd_mailserver_sieve
shell> ansible-galaxy role install vbotka.ansible_lib
```

3) Install collections if necessary

```bash
shell> ansible-galaxy collection install community.general
```

4) Fit variables, for example in vars/main.yml

```bash
shell> editor vbotka.freebsd_mailserver_sieve/vars/main.yml
```

5) Create the playbook and inventory

```yaml
shell> cat freebsd-mailserver-sieve.yml

- hosts: mailserver
  roles:
    - vbotka.freebsd_mailserver
    - vbotka.freebsd_mailserver_sieve
```

```ini
shell> cat hosts
[mailserver]
<mailserver-ip-or-fqdn>
[mailserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=yes
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python3.9
ansible_perl_interpreter=/usr/local/bin/perl
```

6) Install and configure Sieve.

```bash
shell> ansible-playbook freebsd-mailserver-sieve.yml
```

7) Consider to test the mailserver with http://mxtoolbox.com/


## User configuration

1) Create .forward in user home directory

```bash
shell> cat .forward
| "/usr/local/libexec/dovecot/dovecot-lda"
```

2) Create filter /home/user/sieve/managesieve.sieve

```bash
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

```bash
~/Maildir/Spam
```

If this directory shall be visible in the Roundcube webmail create the Spam directory in Roundcube Settings->Folders


## Ansible lint

Use the configuration file *.ansible-lint.local* when running
*ansible-lint*. Some rules might be disabled and some warnings might
be ignored. See the notes in the configuration file.

```bash
shell> ansible-lint -c .ansible-lint.local
```


## References

- [Pigeonhole Sieve Configuration](http://wiki2.dovecot.org/Pigeonhole/Sieve/Configuration)
- [ManageSieve Configuration](http://wiki2.dovecot.org/Pigeonhole/ManageSieve/Configuration)
- [Pigeonhole Sieve examples](http://wiki2.dovecot.org/Pigeonhole/Sieve/Examples)


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.info)
