# network-devices-backup

**Automate backups of network devices configurations using Ansible and Git VCS**

### 1 Create Ansible role for network devices backup

#### 1.1 Cloning this repo

```Shell
git clone git@github.com:DubrovskyEI/network-devices-backup.git
cd network-devices-backup/ 
```

### 2 Create project

#### 2.1 Manually create project in Gitlab

#### 2.2 Clone git repository

##### 2.2.1 Create SSH key for ansible system user

```Shell
sudo mkdir /home/ansible/.ssh
sudo chmod 775 /home/ansible/.ssh/
sudo ssh-keygen -t rsa -b 2048 -C ansible@example.com -f /home/ansible/.ssh/id_rsa
sudo chown -R ansible:ansible /home/ansible/.ssh/
```

##### 2.2.2 Add ansible user's SSH-key to Gitlab

Using SSH-key from file
```Shell
cat /home/ansible/.ssh/id_rsa.pub
```

##### 2.2.3 Clone remote repository from Gitlab

```Shell
cd /opt
sudo git clone git@gitlab.example.com:noc/network-devices-backups.git
sudo chown -R ansible:ansible /opt/network-devices-backups/
sudo chmod 775 /opt/network-devices-backups/
```

##### 2.2.4 Configure git.name and git.email for ansible user in project git config

```Shell
cd /opt/network-devices-backups/
sudo -u ansible git config user.name ansible
sudo -u ansible git config user.email ansible@example.com
```

### 3 Generate and configure password for ansible user

##### Create account for ansible user on tacacs-server/locally on device

### 4 Create systemd units for running playbook every hour

##### 4.1 Put files into /etc/systemd/system/ dir

```Shell
sudo cp roles/network_devices_backup/files/network-devices-backups.* /etc/systemd/system/
```

##### 4.2 Put ansible user password into /etc/environment

```Shell
echo 'ANSIBLE_READONLY_PASSWORD="<ANSIBLE-USER-PASSWORD>"' | sudo tee -a /etc/environment
```

##### 4.3 Reload systemd manager configuration

```Shell
sudo systemctl daemon-reload
```

##### 4.4 Start and enable systemd network-devices-backups.timer unit

```Shell
sudo systemctl start network-devices-backups.timer
sudo systemctl enable network-devices-backups.timer
```

##### References

> [!abstract]+ References
> 1. https://www.packetswitch.co.uk/ncm-ansible-git/
> 2. https://medium.com/@razansaad2/backup-cisco-configuration-by-ansible-709491ba0a53
> 3. https://www.zen-networks.ma/single-post/automation-of-network-equipment-backups-via-gitlab-ansible
> 4. https://www.thierolf.org/blog/2021/cisco-configuration-backup-with-netbox-and-gitlab/
> 5. https://nwmichl.net/2020/03/09/configbackup-with-ansible-git/

