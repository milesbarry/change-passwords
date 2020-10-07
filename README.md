## change-passwords

Playbook to change one or many account passwords. Simply create a file which
containins a dictionary called `accounts` in the following format:

```
accounts:
  bob: bobspassword
  bill: billspassword
  john: johnspassword
```

We will assume that the above example file is called `usernames.yml`

Optionally encrypt the `usernames.yml` file simply by using `ansible-vault`.

`ansible-vault encrypt usernames.yml` and set a password to protect the file.

If you wish to decrypt the file to plain text, simply execute the following:

`ansible-vault decrypt usernames.yml` and enter the password you used.

### Executing the play

`ansible-playbook -i INVENTORY change-passwords.yml -e @usernames.yml`

Note that we use `-e` to load in our dictionary, as it is a file it must be
prepended with `@` 

If your file is encrypted with `ansible-vault` then you must additionally
pass the parameter `--ask-vault-pass`

### Output logs

Upon exit if any hosts have had a user or users password change you will have
a timestamped `.log` file is the current working directory. The format of the
file is like the example below:

`filename: 2020-10-02-20-51-29.log`

```
vm1.mydomain.net:bob,bill
vm2.mydomain.net:bill
```

First field is the `FQDN` followed by a colon and a comma seperated list of any
accounts that have been changed. If no changes to any hosts has been made, no
logfile will be generated.
