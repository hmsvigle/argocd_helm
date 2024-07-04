# argocd_helm
Argocd + Helm templates

# Helm Structure
  * Current helm structure is as below
  * application directory has to be copied inside chart directory to make the tpl syntax work. 
  * copying app directory to chart directory is temporary for every job. 
  ```bash
  |-- README.md
  |-- apps
  |   `-- hello-world
  |       |-- configmap.yaml
  |       `-- values.yaml
  |-- chart
  |   |-- Chart.yaml
  |   |-- hello-world
  |   |   |-- configmap.yaml
  |   |   `-- values.yaml
  |   |-- templates
  |   |   |-- NOTES.txt
  |   |   |-- _helpers.tpl
  |   |   |-- configmap.yaml
  |   |   |-- deployment.yaml
  |   |   `-- service.yaml
  |   `-- values.yaml
  `-- environments
      |-- reg-values.yaml
      `-- stg-values.yaml
  ```
  * helm command that works:
  ```sh
   helm template hello-world chart/ --set appNameGeneric=hello-world -f apps/hello-world/values.yaml -f environments/stg-values.yaml 

---
# Source: hello-world/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: hello-world-configmap
  namespace: default
  labels:
    app: hello-world
data:
  key01: "value-01"
  key02: "value-02"
  global_key01: global_value01
  global_key02: global_value02
---
# Source: hello-world/templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world
  labels:
    helm.sh/chart: hello-world-0.1.0
    app.kubernetes.io/name: hello-world
    app.kubernetes.io/instance: hello-world
    app.kubernetes.io/version: "1.16.0"
    app.kubernetes.io/managed-by: Helm
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app.kubernetes.io/name: hello-world
    app.kubernetes.io/instance: hello-world
---
# Source: hello-world/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
  labels:
    helm.sh/chart: hello-world-0.1.0
    app.kubernetes.io/name: hello-world
    app.kubernetes.io/instance: hello-world
    app.kubernetes.io/version: "1.16.0"
    app.kubernetes.io/managed-by: Helm
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/name: hello-world
      app.kubernetes.io/instance: hello-world
  template:
    metadata:
      labels:
        app.kubernetes.io/name: hello-world
        app.kubernetes.io/instance: hello-world
    spec:
      containers:
        - name: hello-world
          image: "nginx:1.16.0"
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          livenessProbe:
            httpGet:
              path: /
              port: http
          readinessProbe:
            httpGet:
              path: /
              port: http
  ```
#  
