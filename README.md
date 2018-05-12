# toLABaki Admin

This repository contains scripts and configuration files that are being used
to administer toLABaki's services infrastructure. If you are not a toLABaki
admin this is probably not of much use to you, unless you want to use some of
our scripts as an example for yours' (in which case, feel free).

# Encrypted files

This repository contains somewhat sensitive data, so it uses git-crypt to encrypt
certain sensitive files. If you are a toLABaki admin, you should have your GPG key
registered with git-crypt here so that you can unlock.

## Unlocking

1. ```git crypt unlock```
1. Enter your GPG key password or pin in the gpg agent's dialog

## Locking

1. ```git crypt lock```

## Adding a new admin

1. With the repository unlocked: ```git crypt add-gpg-user USERID```
(where USERID is a GPG fingerprint specifier)

**BEWARE** that the above command will **MAKE A COMMIT**, so if you have
unclean working directory, fix it or stash it first.

## Encrypting a file

Add a ``.gitattributes`` file that specifies the encrypted file is to be
routed through the git-crypt filter:

```
myfile		filter=git-crypt diff=git-crypt
```

Make sure to add this in git before adding the file that needs to be
encrypted (they can also be added together, it's the same).

To test whether the filter has succeeded, check the output of
```git crypt status```

## What should be encrypted?

Anything that leaks private and/or deployment-specific information
like IP addresses, ports, which host runs what, passwords, keys, etc...

# Ansible

We are using ansible for administering our services.

## Playbooks

* All playbooks should be in the top level directory.
* No playbook should refer to a host by its host name. Instead, add an alias
in the ``hosts`` file based on the host's role. This is done to avoid exposing
which host actually runs which service in the unencrypted files.

## Testing

* Tests for each playbook should be defined in tests/<playbook_name>
* Normally we test with vagrant:
 * ```vagrant up``` -> everything should be deployed
 * ```vagrant ssh``` -> gain access to the VM and see what ansible did
 * ```vagrant destroy -f``` -> tear down the VM
