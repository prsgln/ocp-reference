
MitziCom 

Choose OpenShift Platform 3.11  

Provision the environment via OPENTLC lab portal 

 ```
   Services → Catalogs → All Services → OPENTLC OpenShift Labs → OpenShift HA Lab
 ```
GUID=b1b3  
  
**Provisioned Environment Hosts**    
    ROLE|HOSTS  
    Bastion host:|bastion.$GUID.example.opentlc.com, bastion.$GUID.internal  
    NFS server:|support1.$GUID.example.opentlc.com, support1.$GUID.internal  
    Load balancer:|loadbalancer.$GUID.example.opentlc.com, loadbalancer1.$GUID.internal  
    3 OpenShift master nodes:|master{1,2,3}.$GUID.example.opentlc.com, master{1,2,3}.$GUID.internal  
    2 OpenShift infrastructure nodes:|infranode{1,2}.$GUID.example.opentlc.com, infranode{1,2}.$GUID.internal  
    3 OpenShift worker nodes:|node{1-2}.$GUID.example.opentlc.com, node{1-2}.$GUID.internal  
    IPA Server:|ipa.shared.example.opentlc.com (shared resource for all students)  

from the ansible *bastion host*

```bash
sudo -i
ansible all --list-hosts
ansible all -m ping
ansible all -m shell -a 'export GUID=`hostname | cut -d"." -f2`; echo "export GUID=$GUID" >> $HOME/.bashrc'  
ansible all -m shell -a 'echo GUID=$GUID'  
``` 
create ansible hostfile usually under /etc/ansible/hosts


Install Docker/Verify Installation  
from bastion  
```bash
ansible nodes -m shell -a 'rpm -V docker-1.13.1'
ansible nodes -m shell -a 'docker --version'
```













