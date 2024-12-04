# Deploy Strimzi using installation files

* create a namespace called `kafka`

    ```shell
    kubectl create namespace kafka
    ```

* Apply the Strimzi install files (`ClusterRoles`, `ClusterRoleBindings` + `CRDs`)

    ```shell
    kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka' -n kafka
    ```

  * CRDs
    * == schemas -- used for the -- CRs (custom resources) -- _Example:_ `Kafka`, `KafkaTopic`, ... --
      * CRs uses
        * manage
          * Kafka clusters,
          * Kafka topics
          * Kafka users
  * `ClusterRoles` and `ClusterRoleBindings` .yaml 
    * by default, | namespace `myproject`
    * `... latest?namespace=kafka` -> these files | `kafka` namespace
  * `-n kafka` -> definitions & configurations / WITHOUT a namespace referenced -> installed | `kafka` namespace
  * Watch deployment of the Strimzi cluster operator
    ```shell
    kubectl get pod -n kafka --watch
    ```
  * operator's log
    ```shell
    kubectl logs deployment/strimzi-cluster-operator -n kafka -f
    ```

* strimzi-cluster-operator
  * watch for NEW CR
  * create the 
    * Kafka cluster,
    * Kafka topics
    * Kafka users

# Create an Apache Kafka cluster

* Create a NEW Kafka CR + 1! node Apache Kafka cluster

    ```shell
    # Apply the `Kafka` Cluster CR file
    kubectl apply -f https://strimzi.io/examples/latest/kafka/kraft/kafka-single-node.yaml -n kafka 
    ```

* check the kafka cluster
  * wait

    ```shell
    kubectl wait kafka/my-cluster --for=condition=Ready --timeout=300s -n kafka 
    ```

  * `kubectl get kafak -n kafka`

# Send and receive messages

* run a simple producer / -- send messages to a -- Kafka topic
  * -> topic is AUTOMATICALLY created

    ```shell
    kubectl -n kafka run kafka-producer -ti --image=quay.io/strimzi/kafka:{{site.data.releases.operator[0].version}}-kafka-{{site.data.releases.operator[0].defaultKafkaVersion}} --rm=true --restart=Never -- bin/kafka-console-producer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic
    
    // Example:
    kubectl -n kafka run kafka-producer -ti --image=quay.io/strimzi/kafka:0.44.0-kafka-3.8.0 --rm=true --restart=Never -- bin/kafka-console-producer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic
    ```

    * type your messages | prompt
    ```shell
    If you don't see a command prompt, try pressing enter.
    
    >Hello Strimzi!
    ```

* create a consumer / read the topic
  * | different terminal, run

    ```shell
    kubectl -n kafka run kafka-consumer -ti --image=quay.io/strimzi/kafka:{{site.data.releases.operator[0].version}}-kafka-{{site.data.releases.operator[0].defaultKafkaVersion}} --rm=true --restart=Never -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic --from-beginning
    
    // Example:
    kubectl -n kafka run kafka-consumer -ti --image=quay.io/strimzi/kafka:0.44.0-kafka-3.8.0 --rm=true --restart=Never -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic --from-beginning
    ```
    * you'll see the messages / produced
    ```shell
    If you don't see a command prompt, try pressing enter.
    
    >Hello Strimzi!
    ```
