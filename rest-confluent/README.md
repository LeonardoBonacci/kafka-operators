# kafka-operators

thanks to https://github.com/confluentinc/confluent-kubernetes-examples/blob/master/hybrid/ccloud-integration/confluent-platform.yaml

```
kubectl explain kafkarestproxies.platform.confluent.io.spec.dependencies.kafka.authentication.jaasConfig

kubectl create ns confluent
kubectl config set-context --current --namespace confluent

kubectl create secret generic rp-secret --from-file=plain.txt -n confluent


kubectl port-forward service/kafkarestproxy 8082:8082 -n confluent


curl -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \
      --data '{"records":[{"value":{"foo":"bar"}}]}' "http://localhost:8082/topics/foo"
```

https://docs.confluent.io/operator/current/co-prepare.html
https://docs.confluent.io/operator/current/co-authenticate.html
https://docs.confluent.io/operator/current/co-configure-misc.html
https://docs.confluent.io/operator/current/co-quickstart.html
https://docs.confluent.io/cloud/current/cp-component/kafka-rest-config.html
