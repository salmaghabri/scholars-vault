# the app 
## run tests locally
![[cidd-2.png]]
## run
```shell
python3 src/run.py
```
on localhost:5000
![[cidd-3.png]]
# create jobs 
## test
```yaml

run_tests:

  stage: test

  image: python:3.9-slim-buster

  before_script:

    - apt-get update && apt-get install make

  script:

    - make test
```
![[cidd-4.png]]
![[cidd-5.png]]
## build
1. create a private dockerhub repo
2. add dockerhub login credentials to gitlab: create secret variables in project settings
![[cidd-6.png]]


3. from this dockerfile
```Dockerfile
FROM python:3.9-slim-buster

  

LABEL Name="Python Flask Demo App" Version=1.4.2

LABEL org.opencontainers.image.source = "https://github.com/benc-uk/python-demoapp"

  

ARG srcDir=src

WORKDIR /app

COPY $srcDir/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

  

COPY $srcDir/run.py .

COPY $srcDir/app ./app

  

EXPOSE 5000

  

CMD ["gunicorn", "-b", "0.0.0.0:5000", "run:app"]
```

4. make sure that the gitlab runner contains docker client and docker daemon (serivces container)
![[cidd-7.png]]

```yaml
	image: docker:20.10.16
	services:
	    - docker:20.10.16-dind
```

8. make sure that the job container and the service container can communicate, and through tls certificate

```yaml
 variables:
    DOCKER_TLS_CERTDIR: "/certs"
```


```yaml

variables:
  IMAGE_NAME: salmaghabri/gitlabcicd
  IMAGE_TAG: python-app-1.0
stages:
  - test
  - build
run_tests:
  stage: test
  image: python:3.9-slim-buster
  before_script:
    - apt-get update && apt-get install make
  script:
    - make test
build_image:
  stage: build
  image: docker:20.10.16
  services:
    - docker:20.10.16-dind
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  before_script:
    - docker login -u $REGISTRY_USER --password $REGISTRY_PASSWORD
  script:
    - docker build -t $IMAGE_NAME:$IMAGE_TAG .
    - docker push $IMAGE_NAME:$IMAGE_TAG
```

> note: we added stages so that we define the order of the execution of the jobs

![[cidd-8.png]]

## deploy stage
Deployment into Kubernetes is simple using a [generic Helm chart for deploying web apps](https://github.com/benc-uk/helm-charts/tree/master/webapp)

```bash
helm repo add benc-uk https://benc-uk.github.io/helm-charts
```
install 
```shell
helm install demopython  benc-uk/webapp --values myapp.yml
```
![[cidd-16.png]]
The options used for the helm chart:

``` yaml
image:

  repository: ghcr.io/benc-uk/python-demoapp

  tag: latest

  pullPolicy: Always

  

service:

  targetPort: 5000

  port: 5000

  

  type: ClusterIP
```

we run these commands to use port forwarding to acess the application of the cluster 
```shell

export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=webapp,app.kubernetes.io/instance=demopython" -o jsonpath="{.items[0].metadata.name}")

kubectl --namespace default port-forward $POD_NAME 8080:5000
```

![[cidd-14.png]]

Then if we open the browser on 127.0.0.1/5000
![[cidd-15.png]]

### prepare deployement server

![[cidd-17.png]]

![[cidd-18.png]]

![[cidd-19.png]]

```json
loginServer": "salacr.azurecr.io"
```

```shell
az aks create --resource-group sal --name aks --location eastus --attach-acr salacr --generate-ssh-keys
```

```bash
└─$ az aks create --resource-group sal --name aks --location eastus --attach-acr salacr --generate-ssh-keys --node-vm-size Standard_B2s
```

![[cidd-20.png]]

![[cidd-21.png]]

![[cidd-22.png]]

![[cidd-23.png]]
