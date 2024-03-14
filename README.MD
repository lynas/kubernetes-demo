# Kubernetes tutorial

## Install lens
- Manage and monitor kubernetes cluster
- `https://k8slens.dev/`

## Run kubernetes cluster locally
- [Linux]
- install minikube
- start minikube by running 
    - `minikube start`
    - Open lens
    - Click on the Catalogue (Top left)
    - minikube should be in the list 
    - Click on minikube to see the minikube cluster in Lens
    - `minikube stop` (to stop minikube)
- [Macos]
- Install docker desktop
- Run docker desktop and go to setting
- Click on Kubernetes
- Enable and Apply & Restart

## Install kubectl
- `https://kubernetes.io/docs/tasks/tools/`

## Create namespace
- `kubectl apply -f namespace.yaml`
- In Lens top right Click on all Namespaces and select `bookstore` namespace to see only resources associated to `bookstore` namespace

## Create Deployment/Pod
- `kubectl apply -f deployment.yaml`
- Delete Deployment/Pod
- `kubectl delete -f deployment.yaml`
#### By default, app deployed in pod can not be accessed from outside
#### To allow access service is needed
#### Secret like DB_PASSWORD can be added in `Secrets` tab under `Config` menu in Lense
#### Then this can be referenced in `deployment.yaml` under `env`

## Create service
- `kubectl apply -f service.yaml`
- Get ip port from Lens service sidebar after selecting service
- Delete service
- `kubectl delete -f service.yaml`

#### At this point app can be used using host:port e.g. `http://bookstore.com:5000`
#### To allow access app only using host ingress is needed. e.g `http://bookstore.com`

## Create ingress controller
- [Linux]
- Locally run `minikube addons enable ingress` 
- Start ingress pointing to the service above

- [Mac via Docker desktop]
- https://kubernetes.github.io/ingress-nginx/deploy/#quick-start
- ```
  helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
  ```
- `kubectl apply -f ingress.yaml`
- Get ingress ip
- `kubectl describe ingress -n bookstore`
- open `/etc/hosts` and add at the bottom 
- `{ip received from above} bookstore.com`
- open browser go to 
- `bookstore.com`

#### Rather than running above command separately all above can be automated using helm / helm chart
#### Before configuring and running helm delete the namespace. It will delete namespace and associated
- deployment/pod
- service
- ingress

## Create helm chart (Kubernetes package manager)
- Install helm
- `https://helm.sh/docs/intro/install/`
- With helm Create directory and file structure 
- `helm create book-store-helm`
- Start the namespace again, This will only create the namespace
- `kubectl apply -f namespace.yaml`
- Run app with one command using helm
- `helm install bookstore book-store-helm`
- Above command needs to run one time after that any modification can be applied with following command
- e.g. changing replica from 1 to 3
- change only `book-store-helm/values.yaml` then run following
- `helm upgrade bookstore book-store-helm`
- open browser go to 
- `bookstore.com`
- You should see same result as before
- To delete helm which deletes all associated resource of bookstore
- `helm delete bookstore`
