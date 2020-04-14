### Estos scripts se utilizan para instalar el nodo master y los nodos tipo workers del cluster de kubernestes

+ install-k8s-master.sh Este scrip, simplemente comprime todos los elementos necesarios para el nodo master
+ k8s-master-certs.sh   Este script Genera los certificados necesarios para nuestro cluster
+ k8s-master-config.sh  Este script genera todos los roles y configuraciones necesarias para el nodo master y los workes

## Por favor descargue

- Kubernetes server - v1.15.11 - [url](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.15.md#v1100)

```console
wget https://dl.k8s.io/v1.15.11/kubernetes-client-linux-amd64.tar.gz
```

- Etcd server - v3.2.18 - [url](https://github.com/coreos/etcd/releases/tag/v3.2.18)

```console
wget https://github.com/coreos/etcd/releases/download/v3.2.18/etcd-v3.2.18-linux-amd64.tar.gz
```

- Flanneld server - v0.10.0 - [url](https://github.com/coreos/flannel/releases/tag/v0.10.0)
```console
wget https://github.com/coreos/flannel/releases/download/v0.10.0/flannel-v0.10.0-linux-amd64.tar.gz
```

### Luego de instalar etcd proceda a ejecutar install-k8s-master.sh

```console
sh install-k8s-master.sh kubernetes-server-linux-amd64.tar.gz k8s-master
```

Luego de ejecutar el script anterios el se encargara de efectuar los cambios necesarios en /etc/kubernetes/apiserver

#### Iniciar el servicio de api k8s：
`systemctl start kube-apiserver.service`

#### Conceder los permisos vinculantes y los roles dentro del cluster
```console
kubectl create clusterrolebinding system:bootstrapper --user=system:bootstrapper --clusterrole=system:node-bootstrapper
```

#### Iniciar el controller-manager
`systemctl start kube-controller-manager.service`

#### Iniciar el scheduler
`systemctl start kube-scheduler.service`


