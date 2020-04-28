# __SSH Setup__

- [__SSH Setup__](#ssh-setup)
- [Setup Links](#setup-links)
- [Accessing Directories](#accessing-directories)
- [RSA Setup](#rsa-setup)
  - [Install Git On Linux Server And Clone From Repo](#install-git-on-linux-server-and-clone-from-repo)
  - [Recursively Copy From One Directory To Another](#recursively-copy-from-one-directory-to-another)
- [SSH Encryption Types](#ssh-encryption-types)
    - [Symmetrical Encryption](#symmetrical-encryption)
    - [Asymmetrical Encryption](#asymmetrical-encryption)
    - [Hashing](#hashing)

---
# Setup Links

[Beginner's Guide To Setting Up SSH On Linux And Testing Your Setup](https://www.makeuseof.com/tag/beginners-guide-setting-ssh-linux-testing-setup/)

[Setup-ssh-for-github/Setup-ssh-on-github.pdf at master · antonykidis/Setup-ssh-for-github · GitHub](https://github.com/antonykidis/Setup-ssh-for-github/blob/master/Setup-ssh-on-github.pdf)

---
# Accessing Directories

`ssh {user}@{host}` *( host = ip-address )*

`UNIX password: ` *(server asks for password)*

- Authentication
  - Passwords are not recommended - can be brute forced
  - RSA is the recommended setup
  -
---
# RSA Setup

 `cd ~/.ssh`

 `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"` *-C = comments*

 -- *enter filename and password*

 -- *generates a public and private rsa_keypair*

 -- *.pub = public key -- never share private key*

 `pbcopy < ~/ssh/id_rsa_....pub` *copy ssh key to clipboard*

 `ssh {user}@{server}` *enter ssh of server*

 `mkdir .ssh` *make directory*

 `ls -a` *view hidden files*

 `cd .ssh` *enter .ssh directory*

 `touch authorized_keys` *create authorized_keys file*


 `nano authorized_keys`  *open authorized_keys in text editor*

 *paste rsa key in authorized_keys*

 `logout`  *logout of server and exit*

 `exit`

 `ssh-add <path to rsa key>` *if 'PERMISSION_DENIED' error occurs*

---
## Install Git On Linux Server And Clone From Repo

`sudo apt-get git`

`git clone git@github.com:<user>/<repo>.git`

---
## Recursively Copy From One Directory To Another

`rsync -av . <user>@<host>:<directory>`

---
# SSH Encryption Types

*Keys are created with a Key Exchange Algorithm*

### Symmetrical Encryption
  - Uses a single key for both encryption and decryption

### Asymmetrical Encryption
  - Contains Public Keys & Private Keys
  - Public keys are sent to host
  - Private keys on clients are used to decrypt returned public keys
  - *Keys are exhanged with the 'Difiie Hellman Key Exchange'*
### Hashing
  - Used in secure shell connections to deter __man in the middle attacks__
  - Completed with HMX - Hash based method authentication codes
  - Uses packet sequence number - runs through hash function - and checks if host hash matches clients hash







