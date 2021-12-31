# kubernetes-challenge-app

This repo is for [DigitalOcean Kubernetes Challenge](https://www.digitalocean.com/community/pages/kubernetes-challenge)

## Install tool

  * [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos): `$ brew install kubectl`
  * [Lens](https://k8slens.dev/): `$ brew install lens`
  * [Helm](https://helm.sh/docs/intro/install#from-homebrew-macos): `$ brew install helm`

## Step 1

Create [DigitalOcean VPC network](https://cloud.digitalocean.com/networking/vpc)

## Step 2

* Create [DigitalOcean Kubernetes Clusters](https://cloud.digitalocean.com/kubernetes/clusters)
* Click Actions Download Config
* `export KUBECONFIG="./kube-challenge-kubeconfig.yaml"`
* `kubectl config get-contexts` or `kubectl get node` check you can connect to DigitalOcean Kubernetes with config setting
* Open [LENS APP](https://k8slens.dev/)
  * add your config file to it and add Clusters, then add to Hotbar
  * you can use this tool to see the metrics
  * more easy to debug
  
## Step 3

* apply every yml file you need
  * `kubectl apply -f rails-app.yml`
  * `kubectl apply -f postgres-secret.yml`
  * `kubectl apply -f postgres.yml`
  * `kubectl apply -f nginx-ingress.yml`
  * `kubectl apply -f rails-svc.yml`
  * `kubectl apply -f issuer.yml`
  
* add kit you miss 
 
  * ingress-nginx: 
    ```
    helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
    helm repo update
    helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enabled=true
    ```
 
  * kubegres: 
    ```
    kubectl apply -f https://raw.githubusercontent.com/reactive-tech/kubegres/v1.15/kubegres.yaml
    ```
  
  * letsencrypt
    ```
    helm repo add jetstack https://charts.jetstack.io
    helm repo update
    helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.6.1 --set installCRDs=true
    ```
## Step 4

  * create port forwarding (ex: 5678) from LENS to setup database
  * connect to database
    * `psql -h localhost -p 5678 -U postgres` check connect
    * create user
      * `createuser -d test_drone -h localhost -p 5678 -U postgres`
    * change user password
      ```
      psql -h localhost -p 5678 -U postgres
      \pasword test_drone
      ```      
    * create database
      * `createdb test_drone_production -O test_drone -h localhost -p 5678 -U postgres`
  
## Result

https://kube.cindyliu923.com/notes
