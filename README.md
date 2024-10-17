# go-kubernetes
golang microservice with configmap and secret 

STEPS Below:

dockerhub image location 
https://hub.docker.com/repository/docker/jadhavrajashri14/go-kubernetes/general 

 
# Local installation of kuernetes cluster :

Install kubectl  with Macports on macOS 

If you are on macOS and using Macports package manager, you can install kubectl with Macports. 

Run the installation command: 

sudo port selfupdate 

sudo port install kubectl 

Test to ensure the version you installed is up-to-date: 

kubectl version --client 

rajashrijadhav-JVKTKGVKW6:go-kubernetes rajashrijadhav$ kubectl version --client 

Client Version: v1.28.2 

Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3


# install minikube – local Kubernetes cluster 
 
https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Farm64%2Fstable%2Fbinary+download 
 
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64 

sudo install minikube-darwin-arm64 /usr/local/bin/minikube 


# Start your cluster 

From a terminal with administrator access (but not logged in as root), run: 

minikube start 

 
Interact with your cluster 

If you already have kubectl installed (see documentation), you can now use it to access your shiny new cluster: 
kubectl get po -A 
rajashrijadhav-JVKTKGVKW6:go-kubernetes rajashrijadhav$ kubectl get po -A 

NAMESPACE              NAME                                        READY   STATUS    RESTARTS        AGE 

default                go-kubernetes-deployment-7dcfb4dd5b-8p6zn   1/1     Running   0               20m 

default                go-kubernetes-deployment-7dcfb4dd5b-gw5zj   1/1     Running   0               20m 

default                go-kubernetes-deployment-7dcfb4dd5b-k62dt   1/1     Running   0               20m 

kube-system            coredns-6f6b679f8f-2xh8x                    1/1     Running   4 (19h ago)     5d1h 

kube-system            etcd-minikube                               1/1     Running   4 (19h ago)     5d1h 

kube-system            kube-apiserver-minikube                     1/1     Running   4 (19h ago)     5d1h 

kube-system            kube-controller-manager-minikube            1/1     Running   4 (19h ago)     5d1h 

kube-system            kube-proxy-q49nj                            1/1     Running   4 (19h ago)     5d1h 

kube-system            kube-scheduler-minikube                     1/1     Running   4 (19h ago)     5d1h 

kube-system            storage-provisioner                         1/1     Running   10 (134m ago)   5d1h 

kubernetes-dashboard   dashboard-metrics-scraper-c5db448b4-6f47c   1/1     Running   4 (19h ago)     5d 

kubernetes-dashboard   kubernetes-dashboard-695b96c756-mw5dc       1/1     Running   8 (134m ago)    5d


Initially, some services such as the storage-provisioner, may not yet be in a Running state. This is a normal condition during cluster bring-up, and will resolve itself momentarily. For additional insight into your cluster state, minikube bundles the Kubernetes Dashboard, allowing you to get easily acclimated to your new environment: 

minikube dashboard 

 
build golang app locally with  

go build 


# create docker image and push to dockerhub 

docker build -t go-kubernetes . 

docker tag go-kubernetes jadhavrajashri14/go-kubernetes:1.1.0 

docker login 

docker push jadhavrajashri14/go-kubernetes:1.1.0 

 

 
followed the link to create a golang microservice  go-kubernetes 
 
https://coding-bootcamps.com/build-containerized-applications-with-golang-on-kubernetes/#p1


# start k8s cluster” 
minikube start 


# create configmap and secret in k8s cluster 
 
kubectl create secret generic apikey --from-literal=API_KEY=123–456 

kubectl create configmap language --from-literal=LANGUAGE=English 
 
kubectl get secret 
rajashrijadhav-JVKTKGVKW6:go-kubernetes rajashrijadhav$ kubectl get secret 

NAME     TYPE     DATA   AGE 

apikey   Opaque   1      117m 

 
rajashrijadhav-JVKTKGVKW6:go-kubernetes rajashrijadhav$ kubectl get configmap 

NAME               DATA   AGE 

kube-root-ca.crt   1      5d1h 

language           1      116m 


# create the deployment 

kubectl apply -f k8s-deployment.yml 

 
rajashrijadhav-JVKTKGVKW6:go-kubernetes rajashrijadhav$ kubectl get deployments 

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE 

go-kubernetes-deployment   3/3     3            3           8m12s 
 

rajashrijadhav-JVKTKGVKW6:go-kubernetes rajashrijadhav$ kubectl get pods 

NAME                                        READY   STATUS    RESTARTS   AGE 

go-kubernetes-deployment-7dcfb4dd5b-8p6zn   1/1     Running   0          9m44s 

go-kubernetes-deployment-7dcfb4dd5b-gw5zj   1/1     Running   0          9m44s 

go-kubernetes-deployment-7dcfb4dd5b-k62dt   1/1     Running   0          9m44s 


# Logs from pod – golang deployment reading configmap and secrets  
 
rajashrijadhav-JVKTKGVKW6:go-kubernetes rajashrijadhav$ kubectl logs -f go-kubernetes-deployment-7dcfb4dd5b-8p6zn 

2024/09/25 11:26:27 API Key: 123–456 

2024/09/25 11:26:27 LANGUAGE: English 

2024/09/25 11:26:27 Starting Server 
 
 
Deployment and visible in minikube dashboard 

