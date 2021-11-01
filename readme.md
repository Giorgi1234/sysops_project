■ Access to machines.

NAT and Host-Only Interface was used to set up the network. In case if you want to SSH to nodes please change network configuration accordingly to Bridged adapter.
================================================================================================================================================================================
Servers are provisioned and details are as follows:

Ubuntu (16.04) has been chosen as linux image due to low size of OS and preisntalled python. 
(PostgreSQL) 9.5.25 version has been used 
python 3.8  version has been used 

IP table and access Info:
Local VM
deployment_machine = 192.168.56.104
user: userver
password: qwerty1
================================================================================================================================================================================
VirtualBox ova files along with procedure guide can be found down below:
Download ova files from the link below:

https://drive.google.com/file/d/11JwOfPDFKx2hp_HYKFzXigZdfk7MYeW_/view?usp=sharing 

================================================================================================================================================================================
Step-by-step Guide to Import ova files:

1. Download and Start Virtual box.
2. Unzip Appliance.rar.
3. Make sure tools → Network → Properties are configured as following:
4. Go to File →Import Appliance 
5. Select Appliance.ova file and import it.
6. Make sure that MAC address policy is set to Include all network adapater MAC addresses. 
 
7. Click Import and wait for finilization. 
================================================================================================================================================================================
■ Steps performed for completion of assignment
================================================================================================================================================================================
1. Installation of SSH on the node.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
server@userver:~$ sudo apt-get install openssh-server
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  ssh-askpass rssh molly-guard monkeysphere
The following NEW packages will be installed:
  openssh-server
0 upgraded, 1 newly installed, 0 to remove and 184 not upgraded.
Need to get 0 B/335 kB of archives.
After this operation, 904 kB of additional disk space will be used.
Preconfiguring packages ...
Selecting previously unselected package openssh-server.
(Reading database ... 179839 files and directories currently installed.)
Preparing to unpack .../openssh-server_1%3a7.2p2-4ubuntu2.10_amd64.deb ...
Unpacking openssh-server (1:7.2p2-4ubuntu2.10) ...
Processing triggers for ureadahead (0.100.0-19.1) ...
Processing triggers for systemd (229-4ubuntu21.28) ...
Processing triggers for ufw (0.35-0ubuntu2) ...
Processing triggers for man-db (2.7.5-1) ...
Setting up openssh-server (1:7.2p2-4ubuntu2.10) ...Saving to: ‘prometheus-2.30.0.linux-amd64.tar.gz.1’
userver@userver:~$ ssh -V
OpenSSH_7.2p2 Ubuntu-4ubuntu2.10, OpenSSL 1.0.2g  1 Mar 2016
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Online Material used: 
https://www.cyberciti.biz/faq/ubuntu-linux-install-openssh-server/ 
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
================================================================================================================================================================================
2. Installation of ansible on the node.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
userver@userver:~$ python -m pip install --user ansible
Collecting ansible
  Downloading ansible-4.7.0.tar.gz (36.0 MB)
     |████████████████████████████████| 36.0 MB 269 kB/s            
  Preparing metadata (setup.py) ... done
Collecting ansible-core<2.12,>=2.11.6
  Downloading ansible-core-2.11.6.tar.gz (7.0 MB)
     |████████████████████████████████| 7.0 MB 1.1 MB/s            
  Preparing metadata (setup.py) ... done
Requirement already satisfied: jinja2 in /usr/lib/python3/dist-packages (from ansible-core<2.12,>=2.11.6->ansible) (2.8)
Collecting PyYAML
  Downloading PyYAML-6.0-cp36-cp36m-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (603 kB)
     |████████████████████████████████| 603 kB 1.0 MB/s            
Requirement already satisfied: cryptography in /usr/lib/python3/dist-packages (from ansible-core<2.12,>=2.11.6->ansible) (1.2.3)
Collecting packaging
  Downloading packaging-21.2-py3-none-any.whl (40 kB)
     |████████████████████████████████| 40 kB 1.2 MB/s            
Collecting resolvelib<0.6.0,>=0.5.3
  Downloading resolvelib-0.5.4-py2.py3-none-any.whl (12 kB)
Requirement already satisfied: pyparsing<3,>=2.0.2 in /usr/lib/python3/dist-packages (from packaging->ansible-core<2.12,>=2.11.6->ansible) (2.0.3)
Building wheels for collected packages: ansible, ansible-core
  Building wheel for ansible (setup.py) ... done
  Created wheel for ansible: filename=ansible-4.7.0-py3-none-any.whl size=59213656 sha256=85fa6267bcdeedca9489174d855ef89b7d216d6c0f3d3770b58b6eb4c1293317
  Stored in directory: /home/userver/.cache/pip/wheels/58/84/c5/bca414814844b72423541f8d2dcc70bca03b9375550fa53c9d
  Building wheel for ansible-core (setup.py) ... done
  Created wheel for ansible-core: filename=ansible_core-2.11.6-py3-none-any.whl size=1950196 sha256=8d29c694f9c7fbb66386b97e3fc5b12ca37f3c5db028b4d66ff02fe029638948
  Stored in directory: /home/userver/.cache/pip/wheels/6b/b5/d3/806dae458c6e0fd75cf408f56ca8ef68c2bb22fea0c9aff73f
Successfully built ansible ansible-core
Installing collected packages: resolvelib, PyYAML, packaging, ansible-core, ansible
Successfully installed PyYAML-6.0 ansible-4.7.0 ansible-core-2.11.6 packaging-21.2 resolvelib-0.5.4
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Online Material used: 
https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html 
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
================================================================================================================================================================================
3. Configure ansible user for root access.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL
userver ALL=(ALL:ALL) NOPASSWD: ALL
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
================================================================================================================================================================================
4. Playbook for nginx and postgresql installation (/opt/playbooks/ nginx-postgres-install.yml): 
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--- 
- 
  become: true
  handlers: ~
  hosts: localhost
  tasks: 
    - 
      apt: 
        cache_valid_time: 3600
        update_cache: true
      name: "apt-get update"
    - 
      apt: 
        name: 
          - nginx
        state: latest
      name: "install nginx"
    - 
      name: "start nginx"
      service: 
        name: nginx
        state: started
    - 
      apt: 
        name: 
          - postgresql
        state: latest
      name: "Install PostgreSQL"
    - 
      name: "start PostgreSQL"
      service: 
        name:  postgresql
        state: started
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

○ Deliverables:
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
userver@userver:/opt/playbooks$ ansible-playbook -c local -i 'localhost,' nginx-postgres-install.yml -K 
BECOME password: 

PLAY [localhost] *******************************************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 16.04 on host localhost should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible 
release will default to using the discovered platform python for this host. See https://docs.ansible.com/ansible-core/2.11/reference_appendices/interpreter_discovery.html for more information. This 
feature will be removed in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [localhost]

TASK [apt-get update] **************************************************************************************************************************************************************************************
ok: [localhost]

TASK [install nginx] ***************************************************************************************************************************************************************************************
ok: [localhost]

TASK [start nginx] *****************************************************************************************************************************************************************************************
ok: [localhost]

TASK [Install PostgreSQL] **********************************************************************************************************************************************************************************
ok: [localhost]

TASK [start PostgreSQL] ************************************************************************************************************************************************************************************
ok: [localhost]

PLAY RECAP *************************************************************************************************************************************************************************************************
localhost                  : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
userver@userver:/opt/playbooks$ ps -ef | grep nginx
userver    562  2061  0 15:04 pts/1    00:00:00 grep --color=auto nginx
root     21356     1  0 11:17 ?        00:00:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
www-data 21357 21356  0 11:17 ?        00:00:00 nginx: worker process
www-data 21358 21356  0 11:17 ?        00:00:00 nginx: worker process
www-data 21359 21356  0 11:17 ?        00:00:00 nginx: worker process
www-data 21360 21356  0 11:17 ?        00:00:00 nginx: worker process
userver@userver:/opt/playbooks$ service nginx status
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since v 2021-10-31 11:17:23 CET; 3h 46min ago
 Main PID: 21356 (nginx)
   CGroup: /system.slice/nginx.service
           ├─21356 nginx: master process /usr/sbin/nginx -g daemon on; master_process on
           ├─21357 nginx: worker process                           
           ├─21358 nginx: worker process                           
           ├─21359 nginx: worker process                           
           └─21360 nginx: worker process                           

okt 31 11:17:23 userver systemd[1]: Starting A high performance web server and a reverse proxy server...
okt 31 11:17:23 userver systemd[1]: Started A high performance web server and a reverse proxy server.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
================================================================================================================================================================================

6. Playbook for setting up the user&pwd for DB.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--- 
-
  name: "Create DB User & Pwd" 
  hosts: localhost
  vars: 
    db_name: user
    db_password: qwerty1
    db_user: userver
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
○ Deliverables:
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
userver@userver:/opt/playbooks$ ansible-playbook -c local -i 'localhost,' create-db-userpwd.yml -K 
BECOME password: 

PLAY [Create DB User & Pwd] ********************************************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 16.04 on host localhost should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible 
release will default to using the discovered platform python for this host. See https://docs.ansible.com/ansible-core/2.11/reference_appendices/interpreter_discovery.html for more information. This 
feature will be removed in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [localhost]

PLAY RECAP *************************************************************************************************************************************************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
================================================================================================================================================================================
7. Install simple Python application which would serve "Hello World!".

In order to set up python web framework server gateway interface using flask it is necessary to set up a virtual environment.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Python script (/python-app/app.py:
from flask import Flask

app = Flask(__name__)
@app.route('/')
def hello_world():
 return "<center>Hello World</center>"

if __name__ == '__main__':
 app.run(debug=True,host='0.0.0.0')
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Test result on web browser http://192.168.0.104:5000/
================================================================================================================================================================================