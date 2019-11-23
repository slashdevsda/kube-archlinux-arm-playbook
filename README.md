# ARM k8s

Setup vanilla kubernetes on Rasperry pi

Tested with Raspberry 3 B+/4 B+, Archlinux ARM. 

Should be mostly distro-agnostic since it essentially fetches precompiled (arm) binaries


# steps:

## Add an ansible user to rasbperries + ssh keys

_replace with your own RSA public key_

```
useradd -m ansible
echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
mkdir /home/ansible/.ssh
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCycKuiAzvLiVjdKvH6RSTZN6laGioS4iBDv4kqSbFer1SercW4vbwZnMeKPBgRj6dOEZclRJn8xfCjsqDRmdfCA4Ew2/QuaEIevKXNJjHIMHI19vxa2Ev0K82h+C4iW/EjWcBg7tasOAEU7EA5bwQ1JdBuZKVZugkaIKyyjV5E0lPbCAzND7DBAlDvXlQQwjkAZ80a4Dyneq8QqYPInWNGei6zLCCGYKfp5VTvTXPHYkyyvF9jeAIJ54q9kzmmoiLj9XcDGZRnT25fyZCj4o/5/QpjXQFam12aDxdDzUNbMAyne0swrQDamKbbU3+7V87eOP33tV+W/gluIuHgrUpOEMM+S3SCL7AU39hcKFJhNNFQ/WA13NUMyw6c9/tnNaytEqUgZ/8X5zaylDdfKmzilGGBJPYN6WMzv6FwuYV6WC1eE/9HGER0ifU0AwcuxR3+1/tSRv44+YqNLZP/0G0uF/L07DFLL8TmmaypJHMp/cQxsXyeNhMxg7ho5fKJ+PAUT98LeulyZSmfWFaPFWyJDRxhniN6dmhXmE+mn0r8h0C2cW/cZWzhFjsFhooTw02gDUitj6HhUdayzccJH849g7pep3UJtXHqFH/BIUNBi7MMp5hlgL6DZWBLSHQfnfeIHsMxEEhMrEle247m7IFc1d3TGckF8wsNLb+HHFCPiw== thomas' > /home/ansible/.ssh/authorized_keys
chmod g-w /home/ansible/.ssh2
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


When your node is ready (`kubectl get nodes`), you can move forward an install *flannel*: 

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```


#### make the master schedulable

- _k8s < 17_
  `kubectl taint node <master-node> node-role.kubernetes.io/master-`

- _k8s >= 17_
  `kubectl taint node <master-node> node-role.kubernetes.io/master:NoSchedule-`


### Worker nodes

At this point, you should be able to add others nodes using `kubeadm join`.
