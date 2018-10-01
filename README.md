# Cheat Sheet

## Installing
  ansible-playbook -i /root/my_hosts /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml 
## Un-Installing
 
  ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/adhoc/uninstall.yml 
  ansible nodes -a "rm -rf /etc/origin"  
  ansible nfs -a "rm -rf /srv/nfs/*"   

 
