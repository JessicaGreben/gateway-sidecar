# gateway-sidecar

This repo contains kubernetes deployment configs and documentation for the Storj S3 gateway.

## S3 Gateway Setup

To setup the Storj S3 gateway follow the setup steps in Storj docs: [S3-gateway docs](https://documentation.tardigrade.io/api-reference/s3-gateway)

Once you reach the ["run the gateway" step](https://documentation.tardigrade.io/api-reference/s3-gateway#run-the-gateway), do the steps below to deploy to kubernetes: 

## Deploy:

### Deploy with kubernetes manifests

```
kubectl create configmap gateway-config --from-file /path/to/gateway/config.yaml

kubectl apply -f manifests/
```

If you want to test the deployment locally, port forward the gateway pod:

```
$ kubectl port-forward <gateway pod> 7777:7777

// the access key and secret will be in the logs in the pod of the gateway
$ export AWS_ACCESS_KEY_ID=<access>;
$ export AWS_SECRET_ACCESS_KEY=<secret>;

$ aws s3 --endpoint=http://localhost:7777/ ls
````

### Deploy with helm

```
kubectl create configmap gateway-config --from-file /path/to/gateway/config.yaml

helm install charts/s3-gateway
```

### Deploy as a sidecar

This is not recommended since the s3 gateway has [high resource requirements](https://documentation.tardigrade.io/api-reference/s3-gateway#minimum-requirements), its better to run as its own deployment.
