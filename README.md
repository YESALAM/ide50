## IDE 50

### Rolling a new version of ide50 deb

(Assumes your CWD is the root of this repo)

After making desired changes:
* Be sure to set `VERSION` in `Makefile` to the new version.
* Add changelog entry to `changelog`.

#### Authentication

(Should be done once)

This package requires cloning some private GitHub repos meanwhile. In order to
be able to build a version of this package, you need to have the correct access
rights to these repos.

You may use existing SSH keys or [generate](https://help.github.com/articles/generating-an-ssh-key/) new ones. Assuming you have a pair of keys:

```
$ mkdir .ssh
$ cp path/to/public_key .ssh
$ cp path/to/private_key .ssh
```

#### Building

```
$ make bash
$ make deb
```

#### Installing

```
sudo -E dpkg --force-confnew --force-confmiss -i ide50_*.deb
```

### Pushing deb to mirror
1. Build new version of deb as described above.
1. `scp ide50_*.deb user@local.cdn.cs50.net:~ # copy the deb to mirror`
1. `ssh user@local.cdn.cs50.net`
1. `sudo su rbowden # rbowden has the script to update the deb in mirror`
1. `cp ide50_*.deb /home/rbowden # copy the deb to rbowden's home folder`
1. `cd # go to rbowden's home`
1. `chown rbowden:producers ide50_*.deb # fix ownership`
1. `sudo bash #prep environment`
1. Finally execute the scrip  that effectively uploads all debs in rbowden's home folder to production. It will ask for gpg password which is in the Heads 1password.
1. `sh deb/deb64.sh`
1. If you want to delete older versions: `cd /srv/www/vhosts/mirror.cs50.net/htdocs/ide50/2015/dists/trusty/main/binary-amd64; rm ide50_{N-1}; push50 -d`
1. Wait a few moments for the mirrors to sync.
1. At this point it's a good idea to test `sudo apt-get update` in the IDE to make sure everything works. Also, logs and errors generated by update50 can be found at `/home/ubuntu/lib/ide50.log`

