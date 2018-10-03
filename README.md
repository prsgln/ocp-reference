# Cheat Sheet
## Using screen
 **On the bastion host**  
 yum install screen  
 screen -S OCP_Training  
 screen -ls  
 screen -r *<session name>*  
 **Inside Screen**  
 Ctrl+d         Detaching  
 Ctrl+a c	new window  
 Ctrl+a n	next window   
 Ctrl+a p	previous window	    
 Ctrl+a \"	select window from list  
 Ctrl+a Ctrl+a	previous window viewed  
 Ctrl+a S	split terminal horizontally into regions  
 Ctrl+a |	split terminal vertically into regions   

## Installing
  ansible-playbook -i /root/my_hosts /usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml   
  ansible-playbook -i /root/my_hosts /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml 
## Un-Installing
 
  ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/adhoc/uninstall.yml  
  ansible nodes -a "rm -rf /etc/origin"  
  ansible nfs -a "rm -rf /srv/nfs/<dirs>" <- specify the dirs like monitorings,loggings etc etc   
  ansible all -m ping  

## Ansible others
  ansible nfs -a "rm -rf /srv/nfs/*"  
  ansible masters[0] -b -m fetch -a "src=/root/.kube/config dest=/root/.kube/config flat=yes"  <- so to run from bastion oc command as system:admin    
  

## Links
 https://www.opentlc.com/labs/ocp_advanced_deployment   
 https://www.opentlc.com/labs/ocp_advanced_deployment/02_1_HA_Deployment_Lab.html#_uninstalling_openshift  
 https://labs.opentlc.com/service/explorer  
 https://docs.openshift.com/container-platform/3.9/install_config/install/advanced_install.html  
 https://docs.openshift.com/container-platform/3.9/install_config/aggregate_logging.html#install-config-aggregate-logging  
 https://docs.google.com/document/d/1sQaWQmBnbxsW6mgNsAoaXErGvRUo9hG-nV_Q5DeUMYc/edit?invite=CLrjxpoG&ts=5bb217ef#heading=h.7lrt9bp4q74y     

## Jourald Logs
 journalctl --since "1 hour ago"     
 journalctl -f  #it means follow   
 journalctl -u sshd.service #it means unit    

