## Playbook description

This playbook can be used for the following

1) Update the kernel for the server
2) Create an LVM volume on /dev/sdb with 80% VG size being used.
3) Create an XFS filesystem on LVM and mount it to /apps/testing

**It's using password authentication to connect to client VMs. Password (SUDO and SSH) are stored in passwd.yml, which is encrypted using ansible-vault**

**ansible_role_update_kernel playbook can found under https://github.com/roshans416/ansible_role_update_kernel.git**

**ansible_role_create_lvm_partition can found under https://github.com/roshans416/ansible_role_create_lvm_partition.git**  

To create password vault

```
$ ansible-vault create passwd.yml
````

Once the vault file is created, add the password variables inside.

```
my_cluster_sudo_pass: **Please copy your password**
```
## Contents:

`master-playbook.yml`: Master playbook which refers to other roles.

`hosts`: Sample inventory. Modify it accordingly.

`config.yml`: Common Vars

`passwd.yml`: Location where the SSH and SUDO passwords are encrypted using ansible-vault

`requirements.yml`: Refers to the roles to be downloaded.

`install_roles.yml` : Installs "update_kernel" role


 
## Variables
```
device_name: /dev/sdb        (Name of device)

vg_name: testVG              (Name of VolumeGroup)

lvm_name: testLVM            (Name of LVM) 
 
mount_point: /apps/testing   (Location to whcih the LVM should be mounted)

#sshd:
#  port: 22022                (SSH port number. Remove this line to fall back to defaiult 22)
#  root_login: yes            (Allow SSH root login. Default is to deny. Delete this line to fall back to default) 
#  pub_key_auth: yes          (Allow SSH PublicKey auth. Default is to deny. Delete this line to fall back to default)
#  password_auth: yes         (Allow password authentication. Default is to deny. Delete this line to fall back to default)
#  use_dns: yes               (Allow hostname resolving)
#  use_pam: yes               (Allow PAM)
```

## How to execute ?

```
$ ansible-playbook install_roles.yml  

$ ansible-playbook -i hosts --ask-vault-pass --extra-vars '@passwd.yml' master-playbook.yml
```
