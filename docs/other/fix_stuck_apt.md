# Apt and Dpkg stuck at install/remove packages
I was about to install docker in groovy (ubuntu 20.10) before the packages were published to the apt repository, I changed the apt list entry to use "focal" (ubuntu 20.04) instead and tried to install as I usually do when packages aren't yet published to a specific ubuntu version, the installation failed and then it stucks at installing/uninstalling docker, no other apt command works it said I should run "sudo dpkg --configure -a" because some packags are not configured then it stuck
there when it tries to mess with docker

### What I did ?
- Remove the packages information of docker-ce* and containerd from /var/lib/dpkg/status
- Remove all related files in `var/lib/dpkg/info/`
- `sudo dpkg --remove --force-remove-reinstreq docker-ce`
