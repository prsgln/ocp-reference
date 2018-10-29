
MitziCom 

provision the environment via OPENTLC lab portal 

 ```
   Services → Catalogs → All Services → OPENTLC OpenShift Labs → OpenShift HA Lab
 ```
GUID=b1b3  
  
Provisioned Environment Hosts
    Bastion host: || bastion.$GUID.example.opentlc.com, bastion.$GUID.internal  
    NFS server: ||  support1.$GUID.example.opentlc.com, support1.$GUID.internal  
    Load balancer: || loadbalancer.$GUID.example.opentlc.com, loadbalancer1.$GUID.internal  
    3 OpenShift master nodes: || master{1,2,3}.$GUID.example.opentlc.com, master{1,2,3}.$GUID.internal  
    2 OpenShift infrastructure nodes: || infranode{1,2}.$GUID.example.opentlc.com, infranode{1,2}.$GUID.internal  
    3 OpenShift worker nodes: || node{1-2}.$GUID.example.opentlc.com, node{1-2}.$GUID.internal  
    IPA Server: || ipa.shared.example.opentlc.com (shared resource for all students)  
