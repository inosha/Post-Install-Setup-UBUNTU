# Post-Install-Setup-UBUNTU

#### 1. time zone adjustment:

  timedatectl set-timezone 'Asia/Colombo' && dpkg-reconfigure --frontend noninteractive 

  tzdata

#### 2. sda continuous errors:

vi /etc/multipath.conf
i
blacklist {
    devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st|sda)[0-9]*"
}

#### 3. assign sudo privileges to a user

 usermod -aG sudo USERNAME
 
#### 4. netplan - static configuration
```

  network:
      ethernets:
          ens192:
              addresses:
             - 1.2.3.4/24
              - 2401:/64
             gateway4: 1.2.3.1
             gateway6: 2401:dd00::
              nameservers:
                addresses:
                - 1.1.1.3
      version: 2
```

#### 5 netdata monitring (optional)

apt install netdata
vi /etc/netdata/netdata.conf

#### 6.  SSH login as root user (optional)

     sed -i --follow-symlinks 's/PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config

    service ssh restart
  
#### 7. NMTUI Network Setup (optional) require for cockpit 

  apt install network-manager
  
  systemctl enable NetworkManager && systemctl start NetworkManager

  vi /etc/netplan/50-cloud-init.yaml
```
  network:
      version: 2
      renderer: NetworkManager
```
#### 8.  User privilege specification: sudo command without a password

Backing up existing: ``` cp /etc/sudoers /root/sudoers.bak ```

giving systemctl : 
visudo
```userid ALL = NOPASSWD: /bin/systemctl restart httpd.service, /bin/kill```

ALL sudo commands without password:
visudo
```userid ALL=(ALL) NOPASSWD:ALL```
***

#### disk expand
  growpart /dev/xvda 1  # Grows the partition; note the space
  
  resize2fs /dev/xvda1  # Grows the filesystem
  

#### view public ip addresses
curl ifconfig.io -4 # -4 for ipv6 -6 for ipv6
