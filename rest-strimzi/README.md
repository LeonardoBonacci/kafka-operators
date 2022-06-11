# rest strimzi

```
kubectl config set-context --current --namespace default
```

## Kafka bridge standalone

```
./bin/kafka_bridge_run.sh --config-file=config/application.properties

curl -X POST \
  http://localhost:8080/topics/foo \
  -H 'content-type: application/vnd.kafka.json.v2+json' \
  -d '{
    "records": [
        {
            "key": "my-key",
            "value": "my-value"
        }
    ]
}'

```

## Or better - k8s sidecar

https://strimzi.io/blog/2021/08/18/using-http-bridge-as-a-kubernetes-sidecar/

```
kubectl apply -f bridge-configmap.yaml
kubectl apply -f bridge-pod.yaml

kubectl exec -ti bridge-sidecar -c main -- bash

curl -X POST http://localhost:8080/topics/foo \
     -H 'Content-Type: application/vnd.kafka.json.v2+json' \
     -d '{ "records": [ { "value": "Hello World!" } ] }'
```

## Or - at last - using k8s crd

```
kubectl create ns kafka
kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka' -n kafka

kubectl explain kafkabridges.kafka.strimzi.io.spec.authentication

echo -n '...' > my-password.txt
kubectl create secret generic bridge-secret --from-file=my-password.txt -n kafka

kubectl apply -f kafka-bridge.yaml -n kafka
kubectl logs -f deployment.apps/my-bridge-bridge -n kafka
kubectl port-forward deployment.apps/my-bridge-bridge 8080:8080 -n kafka

curl -X POST \
  http://localhost:8080/topics/foo \
  -H 'content-type: application/vnd.kafka.json.v2+json' \
  -d '{
    "records": [
        {
            "key": "my-key",
            "value": "last-value"
        }
    ]
}'
```
