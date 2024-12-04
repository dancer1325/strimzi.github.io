# Deleting your Apache Kafka cluster

* 👀if you want to delete ALL Strimzi custom resources (including Apache Kafka cluster) / leave Strimzi cluster operator👀

    ```shell
    kubectl -n kafka delete $(kubectl get strimzi -o name -n kafka)
    ```
  * -> STILL -- can respond to -- NEW Kafka CRs

* delete the PVC / -- was used by the -- cluster

    ```shell
    kubectl delete pvc -l strimzi.io/name=my-cluster-kafka -n kafka
    ```

  * ⚠️if you do NOT delete it -> NEXT Kafka cluster -- might start -- failing ⚠️
    * Reason: 🧠 it will try to use the volume / -- belonged to the -- PREVIOUS Apache Kafka cluster 🧠

# Deleting the Strimzi cluster operator

* if you want to FULLY remove the Strimzi cluster operator + associated definitions -> run

    ```shell
    kubectl -n kafka delete -f 'https://strimzi.io/install/latest?namespace=kafka'
    ```

# Deleting the `kafka` namespace

```shell
kubectl delete namespace kafka
```
