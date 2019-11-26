# ARM k8s

Setup vanilla kubernetes on Rasperry pi

Tested with Raspberry 3 B+/4 B+, Archlinux ARM.

# steps:

## Add an ansible user to rasbperries + ssh keys

open a shell (root) on your rasberry.



You'll need to install python to provide a runtime for ansible, on each _Pi_, along with `sudo`.

_replace with your own RSA public key_

```
useradd -m ansible
echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
mkdir /home/ansible/.ssh
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCycKuiAzvLiVjdKvH6RSTZN6laGioS4iBDv4kqSbFer1SercW4vbwZnMeKPBgRj6dOEZclRJn8xfCjsqDRmdfCA4Ew2/QuaEIevKXNJjHIMHI19vxa2Ev0K82h+C4iW/EjWcBg7tasOAEU7EA5bwQ1JdBuZKVZugkaIKyyjV5E0lPbCAzND7DBAlDvXlQQwjkAZ80a4Dyneq8QqYPInWNGei6zLCCGYKfp5VTvTXPHYkyyvF9jeAIJ54q9kzmmoiLj9XcDGZRnT25fyZCj4o/5/QpjXQFam12aDxdDzUNbMAyne0swrQDamKbbU3+7V87eOP33tV+W/gluIuHgrUpOEMM+S3SCL7AU39hcKFJhNNFQ/WA13NUMyw6c9/tnNaytEqUgZ/8X5zaylDdfKmzilGGBJPYN6WMzv6FwuYV6WC1eE/9HGER0ifU0AwcuxR3+1/tSRv44+YqNLZP/0G0uF/L07DFLL8TmmaypJHMp/cQxsXyeNhMxg7ho5fKJ+PAUT98LeulyZSmfWFaPFWyJDRxhniN6dmhXmE+mn0r8h0C2cW/cZWzhFjsFhooTw02gDUitj6HhUdayzccJH849g7pep3UJtXHqFH/BIUNBi7MMp5hlgL6DZWBLSHQfnfeIHsMxEEhMrEle247m7IFc1d3TGckF8wsNLb+HHFCPiw== thomas' > /home/ansible/.ssh/authorized_keys
chmod g-w /home/ansible/.ssh
chown -R ansible:ansible /home/ansible/.ssh
```

## Read / Configure inventory.ini and site.yaml

Don't add my public key to your nodes

## Apply playbook

```
$ ansible-playbook -i inventory.ini site.yaml
```


## Forming the k8s cluser

### master
An user k8s is created by ansible for these purposes.

Choose a master and ssh on it `ssh k8s@my-node`


```
export PATH="$PATH:/op/bin/"
kubeadm init --pod-network-cidr=10.244.0.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```


```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.1.18:6443 --token 7j3kls.0pwet14cyycz7r37po  --discovery-token-ca-cert-[...]

```

When your node is ready (`kubectl get nodes`), you can move forward an install *flannel*: 

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-legacy.yml -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml
```


#### make the master schedulable

- _k8s < 17 (I think)_ 
  `kubectl taint node <master-node> node-role.kubernetes.io/master-`

- _k8s >= 17_
  `kubectl taint node <master-node> node-role.kubernetes.io/master:NoSchedule-`


### Worker nodes

Try adding nodes using `kubeadm join`.
