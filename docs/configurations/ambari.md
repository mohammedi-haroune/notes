# Apache Ambari

- ambari-agent should be already installed on the hosts
IF NOT installed then
```
Command start time 2019-09-02 03:02:36
env: ‘/var/lib/ambari-agent/tmp/os_check_type1567389754.py’: No such file or directory

Connection to 192.168.33.11 closed.
SSH command execution finished
host=192.168.33.11, exitcode=127
Command end time 2019-09-02 03:02:37

ERROR: Bootstrap of host 192.168.33.11 fails because previous action finished with non-zero exit code (127)
ERROR MESSAGE: Connection to 192.168.33.11 closed.

STDOUT: env: ‘/var/lib/ambari-agent/tmp/os_check_type1567389754.py’: No such file or directory

Connection to 192.168.33.11 closed.
```
- SSh key should not have passphrase
if not then:
```
Permission denied (public key)
```

## Install agent/server
```
sudo wget -O /etc/apt/sources.list.d/ambari.list http://public-repo-1.hortonworks.com/ambari/ubuntu16/2.x/updates/2.7.3.0/ambari.list
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com B9733A7A07513CAD\n
sudo apt update
sudo apt install ambari-agent
sudo apt install ambari-server
```

## Vagrant confiugration: multiple machines and copy ssh key
This will allow access to the machine as the root user

```ruby
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false
    # Customize the amount of memory on the VM:
    vb.memory = "1024"
  end

  config.vm.define "namenode" do |namenode|
    config.vm.network "private_network", ip: "192.168.33.10"
  end

  config.vm.define "datanode1" do |datanode1|
    config.vm.network "private_network", ip: "192.168.33.11"
  end

  config.vm.define "datanode2" do |datanode2|
    config.vm.network "private_network", ip: "192.168.33.12"
  end

  ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa_ambari.pub").first.strip
  config.vm.provision 'shell', inline: 'mkdir -p /root/.ssh'
  config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /root/.ssh/authorized_keys"
  config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys", privileged: false


```
