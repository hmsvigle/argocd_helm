# Tested helm deployment from home directory for reg namespace

````bash
controlplane $ pwd
/root/argocd_helm

controlplane $ helm upgrade --install -n reg  hello-world apps/hello-world/  -f apps/hello-world/values.yaml -f apps/hello-world/environments/reg-values.yaml 
Release "hello-world" does not exist. Installing it now.

NAME: hello-world
LAST DEPLOYED: Wed Jul  3 11:53:30 2024
NAMESPACE: reg
STATUS: deployed
REVISION: 1
TEST SUITE: None

controlplane $ k get po -n reg
NAME                           READY   STATUS    RESTARTS   AGE
hello-world-59d65544b9-lx8f7   1/1     Running   0          5s
````
