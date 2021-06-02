# minikube-nfs

```
sudo apt install nfs-kernel-server nfs-common portmap -y
sudo systemctl status nfs-server
sudo mkdir -p /srv/nfs/mydata
sudo chmod -R 777 nfs/

sudo vi /etc/exports
# /srv/nfs/mydata  *(rw,sync,no_subtree_check,no_root_squash,insecure)

sudo exportfs -rv
# exporting *:/srv/nfs/mydata
showmount -e
# /srv/nfs/mydata  *

kubectl get nodes
minikube ip
# 192.168.49.99 (the ip of minikube)

minikube ssh
# ping localhostip

showmount 192.168.49.1 # the ip of the host which runs the NFS server
# Hosts on 192.168.1.7:

mount -t nfs 192.168.1.7:/srv/nfs/mydata /mnt
mount | grep mydata

# 192.168.1.7:/srv/nfs/mydata on /mnt type nfs4

```

Log into the pod:
```
kubectl exec -it nfs-nginx-6cb55d48f7-q2bvd bash
sudo vi /usr/share/nginx/html/test.html
<h1. this should hopefully work</h1>
```

In the host of the minikube instance:
```
ls /srv/nfs/mydata
cat /srv/nfs/mydata/test.html
<h1>this should hopefully work</h1>
kubectl expose deploy nfs-nginx --port 80 --type NodePort
kubectl get svc
# $ nfs-nginx    NodePort    10.102.226.40   <none>        80:32669/TCP
```

