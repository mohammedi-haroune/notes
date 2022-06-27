# Admin

## Setup the server
Link: https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server

- Create the `git` user and the `.ssh/authorized_keys` file
```bash
sudo adduser git
su git
cd
mkdir .ssh && chmod 700 .ssh
touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```
- Add someone's SSH public key to the `.ssh/authorized_keys` file
```bash
cat /tmp/id_rsa.john.pub >> ~/.ssh/authorized_keys
```
-  Create the repo
```bash
sudo su # Login a user root
mkdir /srv/git
chown git:git /srv/git
sudo su git # Login as user git
cd /srv/git
mkdir project.git
cd project.git
git init --bare
Initialized empty Git repository in /srv/git/project.git/
```

## Sizing a server for git
Link: https://gitolite.com/server-sizing.html

**TL;DR;**

- Git is not hungry on resources and developpers interact with it 2-6 times a day on average
- any descent dual-core CPU can do the work
- A machine with about 512 MB *free* RAM will probaly work fine for most developement style repositories. this means a total of 1 GB.

## Security
- Prevents authorized users from getting a shell
```bash
cat /etc/shells   # see if git-shell is already in there. If not...
which git-shell   # make sure git-shell is installed on your system.
sudo -e /etc/shells  # and add the path to git-shell from last command to this
sudo chsh git -s $(which git-shell) # Set the shell to git-shell for git user
```
- Prevent them from SSH port forwarding by prepending their entry in the `.ssh/authorized_keys` with
```bash
no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa [KEY]
```

- [Top 20 OpenSSH Server Best Security Practices](https://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html)

# Developer
## Generate SSH Keys
```bash
ssh-keygen
```

Then share the public key with the Git server administrator to add you to the `.ssh/authorized_keys` file

```bash
cat .ssh/id_rsa.pub
```
Which will give you something like
```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDX6nKWKYcpdyJtzl1m2exlSafRFFHcV02soXjwplwNht/WJIpRu2yYuTveWPztdHp5RZPxGVURpJHbwENC8v2n4LqTWb2TZNC3hjoOYHx+jUSuGkJRV8TigM5lwlL1vWyuaXxUJKEUwdgHGjJag7ywnRYRga2MaK3PN/Z4NllJaTfPyprrVkLLqHqY1YZSd7y5hnC9JWI+1NKgd1fUoANkKS/FIodcO6/AL7Rx+KNwW47Dq4/IT+6wRH+bMGvKSP7P6QODiSEOF24U444z42YhxX3PyXOXmuspHlCLs06Kd5WnId/KbUsS/mn2edxtGEtdXiw0CR6/jr+A3kxz8M3gsiwlTssvkiqTaz7Gr8lHtbj2AxGxhYSC9jyXeS5ynYQEi3QcubDvRsOP6Va4ihKt7IxPe+XBc8qZVRo9qHKY3T2xMn0EistZnV+HXZfNSV/hiGGRDiQqrIFwzSDEUdM5oodAYFHhNwZbbTAEeeVJAU7GM/n/ZqVt249k3drt5ss= azuread\harounemohammedi@DESKTOP-RR5NSCL
```

## Access the repo
- `project` is the directory where the the project files lives
- `gitserver` is the IP/domain of the Git server
	
### if the project is not a git repo
```bash
cd project
git init
git add .
git commit -m 'Initial commit'
git remote add origin git@gitserver:/srv/git/project.git
git push origin main
```

### if the project is already a git repo with configured remote (Gitlab/Github/AzureDevOps)
```bash
cd project
git remote set-url origin git@gitserver:/srv/git/project.git
git push origin --all
```