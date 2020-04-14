### Procedimiento y scripts que se utilizan para instalar el nodo master y los nodos tipo workers del cluster de kubernetes

+ install-k8s-master.sh Este scrip, simplemente comprime todos los elementos necesarios para el nodo master
+ k8s-master-certs.sh   Este script Genera los certificados necesarios para nuestro cluster
+ k8s-master-config.sh  Este script genera todos los roles y configuraciones necesarias para el nodo master y los workes

### Requerimientos previos en los en el nodo master y en los nodos workers 

- Usando esta configuración por defecto de Kubernetes, kubelet no se inicia si el sistema en los nodos master, o workers  tiene el swap activado. Para desactivar el swap en el host, por favor editar  /etc/fstab y elimina (o comenta) la línea de swap.

```console
 swapoff -a
```

- Crear un archivo para permitir cgroups (aplicar esto en todos los nodos)
- Agregar el archivo en  : /etc/systemd/system/kubelet.service.d/11-cgroups.conf

### Debe contener:

```console
[Service]
CPUAccounting=true
MemoryAccounting=true
```
- Agregar las siguientes reglas en /etc/dorna-firewall/rules.d/010-dorna-essential en nodo master y workers

```console
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p tcp --dport 2380 --jump ACCEPT
-A INPUT -p tcp --dport 2379 --jump ACCEPT
-A INPUT -p tcp --dport 6343 --jump ACCEPT
-A INPUT -p tcp --dport 4001 --jump ACCEPT
#SSH                                                                                                                                                                                    
-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT

```

### Por favor descargue en el nodo master

- Nuestro WORKDIR es /opt/ descargar los siguientes binarios y clone el repositorio con los script de configuración:

- Kubernetes server - v1.15.11 - [url](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.15.md#v1100)

```console
wget https://dl.k8s.io/v1.15.11/kubernetes-client-linux-amd64.tar.gz
```

- Etcd server - v3.2.26 - [url](https://github.com/coreos/etcd/releases/tag/v3.2.26)

```console
wget https://github.com/coreos/etcd/releases/download/v3.2.26/etcd-v3.2.26-linux-amd64.tar.gz
```
###  Configurar etcd

The next steps will get the kube's database/state store working and show it in action.

1. Download and extract etcd and etcdctl:
```console
    wget https://github.com/etcd-io/etcd/releases/download/v3.2.26/etcd-v3.2.26-linux-amd64.tar.gz
    tar -xzf etcd-v3.2.26-linux-amd64.tar.gz
```

2. Instalar etcd y los binarios de etcdctl:
```console
    mv etcd-v3.2.26-linux-amd64/etcd /usr/bin/etcd
    mv etcd-v3.2.26-linux-amd64/etcdctl /usr/bin/etcdctl
```

3. Borrar recursos .tar.gz:

```console
    rm -rf etcd-v3.2.26-linux-amd64 etcd-v3.2.26-linux-amd64.tar.gz
```

4.Iniciar etcd:
```console
    etcd --listen-client-urls http://0.0.0.0:2379 --advertise-client-urls http://localhost:2379 &> /etc/kubernetes/etcd.log &
```

5.verifiacar si todo esta correcto con etcdctl:

```console
    etcdctl cluster-health
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


