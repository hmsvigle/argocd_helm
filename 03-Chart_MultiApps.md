# Multiple applications referring to Single Helm chart

  * helm template: `chart/templates`
  * app List: `chart/apps/app-[01/02/03]`
  * Test helm command as below

````bash
helm upgrade --install -n stg  app-01 chart/  -f chart/apps/app-01/values.yaml -f chart/environments/stg-values.yaml -f chart/apps/app-01/environments/stg-values.yaml 
````
 * 
