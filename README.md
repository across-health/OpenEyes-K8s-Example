# OpenEyes K8s Example Deployment
A very simple example OpenEyes deployment through k8s.

## Create Cluster
In this example I am assuming a local k8s instance using ```kind```, in order to use the ingress controller which will be setup later, the cluster that you create needs to be configured accordingly:

```cat kind-config.yml | kind create cluster --name=openeyes --config=-```


## Apply Secrets
Load all the secrets needed for the deployments, passwords, ssh keys etc.

```kubectl apply -f openeyes-secrets.yml```

## Deploy Database

- ```kubectl apply -f openeyes-db-config.yml``` Load config map for the database
- ```kubectl apply -f openeyes-db-pvc.yml``` Apply the storage claim for the database
- ```kubectl apply -f openeyes-db-deployment.yml``` Create the database deployment
- ```kubectl apply -f openeyes-db-svc.yml``` Create the service for the database

## Deploy OpenEyes
- ```kubectl apply -f openeyes-config.yml``` Load the config map for the OpenEyes container
- ```kubectl apply -f openeyes-deployment.yml``` Create the OpenEyes deployment
- ```kubectl apply -f openeyes-svc.yml``` Create the service for OpenEyes

## Deploy Ingress Controller
For this example I am assuming a local k8s instance using ```kind``` so we load the nginx ingress deployment for kind:

```kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml```

Can run the following to wait until the ingress deployment is ready:

```kubectl wait --namespace ingress-nginx   --for=condition=ready pod   --selector=app.kubernetes.io/component=controller   --timeout=90s```

I am using nginx in the example here but you can use traefik instead to have better parity with Toukan's deployments.

## Access
The OpenEyes instance will now be available on localhost.
