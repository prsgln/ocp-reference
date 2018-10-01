# Cheat Sheet
## Using screen
 yum install screen  
 screen -S OCP_Training  
 *CTRL+a;CTRL+d -> detaching*  
 screen -ls  
 screen -r *<session name>*  
 


## Installing
  ansible-playbook -i /root/my_hosts /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml 
## Un-Installing
 
  ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/adhoc/uninstall.yml  
  ansible nodes -a "rm -rf /etc/origin"  
  ansible nfs -a "rm -rf /srv/nfs/*"   

 
