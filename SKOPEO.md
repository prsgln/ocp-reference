# SKOPEO CHEATSHEET
 A complete description of skopeo is available [here](https://github.com/containers/skopeo)
- yum install  podman skopeo jq docker-distribution
- skopeo inspect docker://docker.io/fedora
- skopeo copy --dest-tls-verify=false docker://docker.io/nginx:latest docker://localhost:5000/ngix:1.0 
- skopeo  delete --tls-verify=false docker://localhost:5000/ngix:1.0
- skopeo inspect --tls-verify=false docker://localhost:5000
