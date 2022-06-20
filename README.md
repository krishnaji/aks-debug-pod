# aks-debug-pod
⚠️ Privileged container, with host namespace and networking

```bash
kubectl apply -f debug.yaml
```

```bash
kubectl get pods 

NAME                     READY   STATUS    RESTARTS   AGE
debug-755c8578cf-hr6px   1/1     Running   0          5m9s
```

```bash
kubectl  exec -it debug-755c8578cf-hr6px -- bash

root@aks-nodepool1-300078804-vmss000000:/# 
```

```bash
# host file is mounted at /host

root@aks-nodepool1-300078804-vmss000000:/# ls /host

NOTICE.txt  boot  etc   initrd.img      lib    lost+found  mnt  proc  run   srv  tmp  var      vmlinuz.old
bin         dev   home  initrd.img.old  lib64  media       opt  root  sbin  sys  usr  vmlinuz
```

```bash
# Enter host namespace
root@aks-nodepool1-300078804-vmss000000:/# /usr/bin/nsenter -m/proc/1/ns/mnt sh

# 
```

```bash
# #check host iptables

# iptables -t nat -nvL KUBE-SERVICES

Chain KUBE-SERVICES (2 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 KUBE-SVC-NPX46M4PTMTKRN6Y  tcp  --  *      *       0.0.0.0/0            10.240.0.1           /* default/kubernetes:https cluster IP */ tcp dpt:443
    0     0 KUBE-SVC-ERIFXISQEP7F7OF4  tcp  --  *      *       0.0.0.0/0            10.240.0.2           /* kube-system/kube-dns:dns-tcp cluster IP */ tcp dpt:53
    0     0 KUBE-SVC-TCOU7JCQXEZGVUNU  udp  --  *      *       0.0.0.0/0            10.240.0.2           /* kube-system/kube-dns:dns cluster IP */ udp dpt:53
    2   120 KUBE-SVC-QMWWTXBG7KFJQKLO  tcp  --  *      *       0.0.0.0/0            10.240.0.132         /* kube-system/metrics-server cluster IP */ tcp dpt:443
    5   300 KUBE-NODEPORTS  all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* kubernetes service nodeports; NOTE: this must be the last rule in this chain */ ADDRTYPE match dst-type LOCAL
# 
