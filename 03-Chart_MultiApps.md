# Multiple applications referring to Single Helm chart

  * helm template: `chart/templates`
  * app List: `chart/apps/app-[01/02/03]`
  * Test helm command as below
  ````yaml
  helm upgrade --install -n stg  app-01 chart/  -f chart/apps/app-01/values.yaml -f environments/stg-values.yaml -f chart/apps/app-01/environments/stg-values.yaml 
  ````
  * Result 

  ````yaml
   Release "app-01" has been upgraded. Happy Helming!
   NAME: app-01
   LAST DEPLOYED: Thu Jul  4 06:06:18 2024
   NAMESPACE: stg
   STATUS: deployed
   REVISION: 2
   TEST SUITE: None

  ````
  * Test with additional inputs for app-02
  ````yaml
  helm upgrade  --install -n stg  app-02 chart/  -f chart/apps/app-02/values.yaml -f environments/stg-values.yaml -f chart/apps/app-02/environments/stg-values.yaml --set appName='app-02' --set image.tag='01' --set appNameGeneric=app-02
  ````

  ```bash
    Release "app-02" does not exist. Installing it now.
    NAME: app-02
    LAST DEPLOYED: Thu Jul  4 06:18:51 2024
    NAMESPACE: stg
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
  ```
  * list of apps with single helm template - Tested
    * Application Structure is below format
    ````yaml
    -- chart
        |-- Chart.yaml
        |-- apps
        |   |-- app-01
        |   |   |-- configmap.yaml
        |   |   |-- environments
        |   |   |   |-- reg-values.yaml
        |   |   |   `-- stg-values.yaml
        |   |   `-- values.yaml
        |   `-- app-02
        |       |-- configmap.yaml
        |       |-- environments
        |       |   |-- reg-values.yaml
        |       |   `-- stg-values.yaml
        |       `-- values.yaml
        |-- environments
        |   |-- reg-values.yaml
        |   `-- stg-values.yaml
        |-- templates
        |   |-- _helpers.tpl
        |   |-- configmap.yaml
        |   |-- deployment.yaml
        |   `-- service.yaml
        `-- values.yaml
    ````
    * For `app-01`, 2 scenarios tested i.e deployment to stg & reg.
    ````yaml
    controlplane $ k get applications -n argocd
      NAME         SYNC STATUS   HEALTH STATUS
      app-01-reg   Synced        Healthy
      app-01-stg   Synced        Healthy
    controlplane $ k get po -n stg
      NAME                                 READY   STATUS    RESTARTS   AGE
      app-01-deployment-66ff49b597-vsjq7   1/1     Running   0          17m
      controlplane $ k get po -n reg
      NAME                                 READY   STATUS    RESTARTS   AGE
      app-01-deployment-66ff49b597-mp4b2   1/1     Running   0          42m
    ````

    * 