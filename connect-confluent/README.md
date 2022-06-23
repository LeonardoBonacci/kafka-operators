# Kafka Connect

```
kubectl config set-context --current --namespace confluent
```

thanks to https://docs.confluent.io/operator/current/co-configure-connect.html#co-use-prebuilt-connector-image

```
docker build -t leonardobonacci/mongo-connect:7.1.0 .
docker push leonardobonacci/mongo-connect:7.1.0
```

and https://github.com/confluentinc/confluent-kubernetes-examples/blob/master/hybrid/ccloud-integration/confluent-platform.yaml

certificats:
```
openssl genrsa -out ca-key.pem 2048

openssl req -new -key ca-key.pem -x509 \
  -days 1000 \
  -out ca.pem \
  -subj "/C=US/ST=CA/L=MountainView/O=Confluent/OU=Operator/CN=TestCA"

kubectl create secret tls ca-pair-sslcerts \
    --cert=ca.pem \
    --key=ca-key.pem  
```

confluent cloud credentials
```
kubectl explain connects.platform.confluent.io.spec.dependencies.kafka.authentication.jaasConfig

kubectl create ns confluent
kubectl config set-context --current --namespace confluent

kubectl create secret generic cloud-plain \
  --from-file=plain.txt=creds-client-kafka-sasl-user.txt

kubectl create secret generic kafka-client-config-secure \
  --from-file=kafka.properties \
  -n confluent
```

deploy connector, insert some records and consume...
```
kubectl explain connectors.platform.confluent.io.spec.dependencies.kafka.authentication.jaasConfig

./bin/kafka-console-consumer --bootstrap-server abc.gcp.confluent.cloud:9092 --consumer.config cloud.properties --topic foo.bar --property "print.key=true"
```
