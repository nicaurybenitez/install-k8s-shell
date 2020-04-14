# Este script se utiliza para instalar el nodo master y Nodos tipo Workers , para el entorno Devs habitual

+ install-k8s-master.sh Script principal, simplemente comprime todos los elementos necesarios para  el nodo master
+ k8s-master-certs.sh   Este Script Genera los certificados necesarios para nuestro cluster
+ k8s-master-config.sh  Este script genera todos los roles y configuraciones necesarias para el nodo master

# Primero debe Instalar etcd y luego proceder a ejecutar install-k8s-master.sh

```console
sh install-k8s-master.sh kubernetes-server-linux-amd64.tar.gz k8s-master
```

运行脚本后把 /etc/kubernetes/apiserver 文件中的etcd修改一下

#### 启动k8s：
`systemctl start kube-apiserver.service`

#### 绑定权限
```console
kubectl create clusterrolebinding system:bootstrapper --user=system:bootstrapper --clusterrole=system:node-bootstrapper
```

#### 启动controller-manager
`systemctl start kube-controller-manager.service`

#### 启动scheduler
`systemctl start kube-scheduler.service`


