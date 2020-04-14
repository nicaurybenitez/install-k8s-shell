### Instalación de los nodos workers


### Preparar los siguientes archivos antes de ejecutar el script k8s-install-kubelet.sh
+ ca.crt
+ bootstrap.conf

##### Ejecute el siguiente comando en el nodo maestro para obtener el archivo bootstrap.conf
```console
kubectl config --kubeconfig=bootstrap.conf set-cluster kubernetes --server="https://k8s-master:6443" --certificate-authority=/etc/kubernetes/cert/ca.crt
kubectl config --kubeconfig=bootstrap.conf set-credentials system:bootstrapper --token=54c451.b68dc21e45c57e2a
kubectl config --kubeconfig=bootstrap.conf set-context system:bootstrapper@kubernetes --user=system:bootstrapper --cluster=kubernetes
kubectl config --kubeconfig=bootstrap.conf use-context system:bootstrapper@kubernetes
```
Plugin cni：https://github.com/containernetworking/plugins/releases



### Preparar los siguientes archivos antes de ejecutar el script k8s-install-kubelet.sh
+ kube-proxy.conf

##### Ejecute el siguiente comando en el nodo maestro para obtener el archivo kube-proxy.conf
```console
kubectl config --kubeconfig=kube-proxy.conf set-cluster kubernetes --server="https://k8s-master:6443" --certificate-authority=/etc/kubernetes/cert/ca.crt --embed-certs=true
kubectl config --kubeconfig=kube-proxy.conf set-credentials system:kube-proxy --client-certificate=/etc/kubernetes/cert/kube-proxy.crt --client-key=/etc/kubernetes/cert/kube-proxy.key --embed-certs=true
kubectl config --kubeconfig=kube-proxy.conf set-context system:kube-proxy@kubernetes --cluster=kubernetes --user=system:kube-proxy
kubectl config --kubeconfig=kube-proxy.conf use-context system:kube-proxy@kubernetes
```
#### Instalando el comoponente de red interno con flannel
```console
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
