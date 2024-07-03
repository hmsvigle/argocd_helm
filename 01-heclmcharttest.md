# Test Helm Chart for application
  * Run helm command from `apps/hello-world` directory
  * Created 3 namespaces for test i.e `stg`,`reg`,`prod`
  ```bash
  controlplane $ helm upgrade --install -n stg  hello-world ./ -f values.yaml -f ./environments/stg-values.yaml   
    Release "hello-world" does not exist. Installing it now.
    NAME: hello-world
    LAST DEPLOYED: Wed Jul  3 11:36:49 2024
    NAMESPACE: stg
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
  controlplane $ k get po -n stg
  NAME                           READY   STATUS    RESTARTS   AGE
  hello-world-59d65544b9-qc6d5   1/1     Running   0          8s
  ```
  * `appName = hello-world` updated in values.yaml. we can oveeride it in helm command as well
  * environment wise global values.yaml is being referred from apps/hello-world/environments/<env>-values.yaml

