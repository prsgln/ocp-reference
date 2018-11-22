
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



preparing *ansible bastion host*  
```bash
yum install wget git net-tools bind-utils yum-utils \  
iptables-services bridge-utils bash-completion \  
kexec-tools sos psacct \  
ansible-2.6.5-1.el7ae.noarch \ 
openshift-ansible-playbooks-3.11.16-1.git.0.4ac6f81.el7.noarch \  
openshift-ansible-docs-3.11.16-1.git.0.4ac6f81.el7.noarch \  
openshift-ansible-3.11.16-1.git.0.4ac6f81.el7.noarch \  
openshift-ansible-roles-3.11.16-1.git.0.4ac6f81.el7.noarch \  
atomic-openshift-clients-3.11.16-1.git.0.b48b8f8.el7.x86_64 \  
atomic-openshift-3.11.16-1.git.0.b48b8f8.el7.x86_64   
```

from the ansible *bastion host*
```bash
sudo -i
ansible all --list-hosts
ansible all -m ping
ansible all -m shell -a 'export GUID=`hostname | cut -d"." -f2`; echo "export GUID=$GUID" >> $HOME/.bashrc'  
ansible all -m shell -a 'echo GUID=$GUID'  
``` 
create ansible hostfile usually under /etc/ansible/hosts


Install/Verify Docker Installation from bastion and assess configured repo  
```bash
ansible -i hosts_file nodes -m yum  -a 'name=docker-1.13.1'
ansible -i hosts_file nodes -m shell -a 'rpm -V docker-1.13.1'
ansible -i hosts-file  nodes -m yum -a 'list=docker-1.13.1'
ansible nodes -m shell -a 'docker --version'
ansible all  -m shell -a"yum repolist" 
ansible  localhost -m shell -a"yum repolist" 
```

Verify NFS export 
```bash
ansible nfs -m shell -a"exportfs"
```
Verify Prerequisites & Install
```bash
 ansible-playbook -i mitzicom_ansible_hosts -f 20 /usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml
 ansible-playbook -i mitzicom_ansible_hosts -f 20 /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml 
```

###Appendix 1  
Changing network plugin  
```bash
  ansible masters -m shell -a "sed -i -e 's/openshift-ovs-subnet/openshift-ovs-multitenant/g'  /etc/origin/master/master-config.yaml"  
  ansible nodes -m shell -a "sed -i -e 's/openshift-ovs-subnet/openshift-ovs-multitenant/g'  /etc/origin/node/node-config.yaml"  
  ansible nodes -m shell -a "grep subnet  /etc/origin/node/node-config.yaml"  
  ansible masters -m shell -a "/usr/local/bin/master-restart api"  
  ansible masters -m shell -a "/usr/local/bin/master-restart controllers"  
```

###Appendix 2  
NFS Persistent Volume Recycling  
For persistent volumes with type recycling OpenShift does no longer provide the recycler pod automatically since it is deprecated.   
This pod however is needed to properly re-use persistent volumes.  The container image still exists with tag latest in the Red Hat registry.   
It only needs to be pulled to all the nodes and made available for use by tagging the image with the exact version installed (note that the version can change in the future):  
```bash
ansible nodes -m shell -a "docker pull registry.access.redhat.com/openshift3/ose-recycler:latest"  
ansible nodes -m shell -a "docker tag registry.access.redhat.com/openshift3/ose-recycler:latest registry.access.redhat.com/openshift3/ose-recycler:v3.9.14"  
```
