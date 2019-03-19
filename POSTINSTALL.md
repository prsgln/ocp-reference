# THIS is the post-installation configuration for NFS
 On NFS server   
```bash
  mkdir -p /srv/nfs/user-vols/pv{1..200}  
  
  for pvnum in {1..50} ; do  
   echo /srv/nfs/user-vols/pv${pvnum} *(rw,root_squash) >> /etc/exports.d/openshift-uservols.exports  
   chown -R nfsnobody.nfsnobody  /srv/nfs  
   chmod -R 777 /srv/nfs  
  done  
    
  systemctl restart nfs-server  
```

 On Bastion  

```bash
 export GUID=asdf  
 export volsize="5Gi"  
 mkdir /root/pvs  
 for volume in pv{1..25} ; do  
  cat << EOF > /root/pvs/${volume}  
  {  
   "apiVersion": "v1",  
   "kind": "PersistentVolume",  
   "metadata": {  
     "name": "${volume}"  
   },  
   "spec": {  
     "capacity": {  
         "storage": "${volsize}"  
     },  
     "accessModes": [ "ReadWriteOnce" ],  
     "nfs": {  
         "path": "/srv/nfs/user-vols/${volume}",  
         "server": "support1.${GUID}.internal"  
     },  
     "persistentVolumeReclaimPolicy": "Recycle"  
   }  
  }  
 EOF  
 echo "Created def file for ${volume}";  
 done;  
```
 Again 25 pv
```bash
 export volsize="10Gi"  
 for volume in pv{26..50} ; do  
 cat << EOF > /root/pvs/${volume}  
 {  
   "apiVersion": "v1",  
   "kind": "PersistentVolume",  
   "metadata": {  
     "name": "${volume}"  
   },  
   "spec": {  
     "capacity": {  
         "storage": "${volsize}"  
     },  
     "accessModes": [ "ReadWriteMany" ],  
     "nfs": {  
         "path": "/srv/nfs/user-vols/${volume}",  
         "server": "support1.${GUID}.internal"  
     },  
     "persistentVolumeReclaimPolicy": "Retain"  
   }  
 }  
 EOF  
 echo "Created def file for ${volume}";  
 done;  
```
 Then you can create the PVs out of the manifests
```bash
 cat /root/pvs/* | oc create -f -  
 oc get pv  
```
