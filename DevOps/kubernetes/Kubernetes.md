# 1. Installation and Configuration of Kubernetes

- Install and configure a local or online Kubernetes cluster.
- Verify that the cluster is operational using the kubectl command.
![[K8s-1.png]]
```shell
kubectl config view
```

![[K8s-16.png]]

# 2. Application Deployment

- Create a Kubernetes deployment for the web application using a YAML configuration file.

```Yaml
apiVersion: apps/v1

kind: Deployment

metadata:

  name: first-deployment

spec:

  replicas: 1

  selector:

    matchLabels:

      app: spring-app

  template:

    metadata:

      labels:

        app: spring-app

    spec:

      containers:

        - name: spring-app

          image: salmaghabri/spring-app

          ports:

            - containerPort: 8080
```
![[K8s-2.png]]
- Verify that the deployment is done correctly and that the pods are running.
![[K8s-5.png]]
![[K8s-6.png]]

# 3. Replica Management
- Scale the application by modifying the number of replicas of the deployment.
```shell
kubectl scale deployment first-deployment --replicas=3

```
![[K8s-7.png]]
- Verify that the new replicas are created and deployed correctly.
![[K8s-8.png]]

# 4. Service Definition
- Create a Kubernetes service to expose the application internally.

```yaml
apiVersion: v1

kind: Service

metadata:

  name: spring-app-service

spec:

  selector:

    app: spring-app

  ports:

    - protocol: TCP

      port: 80

      targetPort: 8080
```


- `selector` specifies the pods targeted by this service. It matches the pods labeled with `app: spring-app`, which is the same label used in your deployment.
- `ports` specifies the ports exposed by the service. In this example, the service exposes port 80 on the cluster, which is forwarded to port 8080 of the pods.

``` shell

kubectl apply -f service.yaml

```

![[K8s-9.png]]


- Verify that the service is accessible from the Kubernetes cluster.
![[K8s-10.png]]

the output shows that the `spring-app-service` service is currently routing traffic to three different pods (`10.1.0.11:8080`, `10.1.0.12:8080`, and `10.1.0.13:8080`), all of which are listening on port `8080`. This suggests that our Spring application is being served by multiple pods and is ready to receive traffic.


# 5. External Application Exposure:

- Define a LoadBalancer type service to expose the application externally.

``` yaml
apiVersion: v1

kind: Service

metadata:

  name: spring-app-loadbalancer

spec:

  type: LoadBalancer

  ports:

    - protocol: TCP

      port: 80

      targetPort: 8080

  selector:

    app: spring-app
```


- Verify that the application can be accessed from outside the cluster using the assigned IP address. Once the LoadBalancer type service is created, we can check its status and get the assigned IP address. Service information, including its type, exposed ports, and the IP address assigned by the LoadBalancer.

```bash
kubectl get svc spring-app-loadbalancer
```
![[K8s-11.png]]


On our machine we can access the app via `http://localhost:80` if we send a get request to `/hello`

![[K8s-12.png]]

# 6. Update Management

### Update the container image used by the application.

We change the image to nginx for example:

 ```
apiVersion: apps/v1

kind: Deployment

metadata:

  name: first-deployment

spec:

  replicas: 1

  selector:

    matchLabels:

      app: spring-app

  template:

    metadata:

      labels:

        app: spring-app

    spec:

      containers:

        - name: spring-app

          image: nginx // image changed

          ports:

            - containerPort: 80
``` 

### Verify that the new version of the application is deployed correctly without service interruption.
![[K8s-13.png]]

while the container is created, the old one is still listening on `localhost:80`
![[K8s-14.png]]



# 7. Monitoring and Logging: Prometheus
[source](https://www.pluralsight.com/cloud-guru/labs/aws/kubernetes-monitoring-with-prometheus)

[kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) (KSM) is a simple service that listens to the Kubernetes API server and generates metrics about the state of the objects deployed in the cluster.
Kube State Metrics is commonly used in conjunction with Prometheus, a popular open-source monitoring and alerting system.
### Install `kube-state-metrics`

```shell
git clone https://github.com/kubernetes/kube-state-metrics.git

```

```shell
kubectl apply -f kubernetes

```
![[K8s-17.png]]

##  Set Up `kube-state-metrics` to Expose Metrics for Your Kubernetes Cluster
We create the`kube-state-metrics` service
```yml
kind: Service
apiVersion: v1
metadata:
  namespace: kube-system
  name: kube-state-nodeport
spec:
  selector:
    k8s-app: kube-state-metrics
  ports:
  - protocol: TCP
    port: 8080
    nodePort: 30000
  type: NodePort

```

```shell
kubectl apply -f kube-state-metrics-nodeport-svc.yml
```

![[K8s-18.png]]
If we open the browser and go to `localhost:30000` we can see the data of our cluster
![[K8s-19.png]]


![[K8s-20.png]]


## Configure Prometheus to Scrape Metrics from `kube-state-metrics`

-  create a promotheus.yaml

```yml

# my global config

global:

  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.

  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.

  # scrape_timeout is set to the global default (10s).



# Alertmanager configuration

alerting:

  alertmanagers:

    - static_configs:

        - targets:

          # - alertmanager:9093

  

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.

rule_files:

  # - "first_rules.yml"

  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:

# Here it's Prometheus itself.

scrape_configs:

  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.

  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'

    # scheme defaults to 'http'.

    static_configs:

      - targets: ["localhost:9090"]

    - job_name: kubernetes

    static_configs:

      - targets: ["localhost:30000"]
```

##### Example of a query
```
kube_pod_status_ready{namespace="default",condition="true"}
```
![[K8s-21.png]]

---
