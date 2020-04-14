### Estos scripts se utilizan para instalar el nodo master y los nodos tipo workers del cluster de kubernestes

+ install-k8s-master.sh Este scrip, simplemente comprime todos los elementos necesarios para el nodo master
+ k8s-master-certs.sh   Este script Genera los certificados necesarios para nuestro cluster
+ k8s-master-config.sh  Este script genera todos los roles y configuraciones necesarias para el nodo master

### Primero debe Instalar etcd y luego proceder a ejecutar install-k8s-master.sh

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


