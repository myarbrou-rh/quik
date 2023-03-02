## Create dev kafka env
oc apply -f https://raw.githubusercontent.com/myarbrou-rh/quik/main/kafka/crw-kafka.yaml

# need this for external svc https://www.confluent.io/blog/kafka-listeners-explained/
oc set env deploy/kafka KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://kafka.$(oc project -q).svc:9092

# test it
oc exec deploy/kafka -- bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test
oc exec deploy/kafka -- /bin/bash -c "date | bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test"
oc exec deploy/kafka -- bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning --max-messages 1
oc delete  -f https://raw.githubusercontent.com/myarbrou-rh/quik/main/kafka/crw-kafka.yaml
