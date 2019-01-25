# Command #

## Apache2 load module ##
- Build in: `apachectl -t -D DUMP_MODULES`
- Directive **LoadModule**: `apachectl -l`

## Setup with zsh ##
```
sudo vi /etc/yum.repos.d/redhat-rhui.repo
sudo vi /etc/selinux/config 
sudo yum update -y && sudo yum install -y docker git zsh
sudo systemctl enable docker
sudo sed -i 's/ec2-user:\/bin\/bash/ec2-user:\/bin\/zsh/1' /etc/passwd
sudo dd if=/dev/zero of=/var/swap bs=1024 count=1024000
sudo chmod -R 600 /var/swap
sudo mkswap /var/swap
sudo swapon /var/swap
sudo echo "/var/swap swap swap defaults 0 0" >> /etc/fstab
sudo reboot
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | zsh
```

## APR based Apache Tomcat Native ##
[CentOS 7 EPEL x86_64](https://centos.pkgs.org/7/epel-x86_64/tomcat-native-1.2.17-1.el7.x86_64.rpm.html)
```
yum install -y apr
tcnative=tomcat-native-1.2.17-1.el7.x86_64.rpm
curl -OL http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/t/${tcnative}
rpm -ivh ${tcnative}
rm -rf ${tcnative}
```

## Mysql ##
```
CREATE DATABASE dbname DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
GRANT ALL PRIVILEGES ON dbname.* to 'myuser'@'localhost';
FLUSH PRIVILEGES;
```

## docker-compose ##
```
curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

## Install docker in RedHat7.5@AWS ##
Edit `/etc/yum.reop.d/redhat-rhui.repo`
```
[rhui-REGION-rhel-server-extras]
name=Red Hat Enterprise Linux Server 7 Extra(RPMs)
mirrorlist=https://rhui2-cds01.REGION.aws.ce.redhat.com/pulp/mirror/content/dist/rhel/rhui/server/7/$releasever/$basearch/extras/os
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
sslverify=1
sslclientkey=/etc/pki/rhui/content-rhel7.key
sslclientcert=/etc/pki/rhui/product/content-rhel7.crt
sslcacert=/etc/pki/rhui/cdn.redhat.com-chain.crt
```
- Change `enabled=0` to `enabled=1`
- Save
- `yum install docker`
