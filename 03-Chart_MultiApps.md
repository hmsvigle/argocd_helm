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
  * 