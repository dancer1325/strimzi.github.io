* see [kind](https://github.com/dancer1325/kind)
  * install it

# Installing the dependencies

* install [`kubectl`](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* validate ALL tools are running

    ```shell
    # Validate docker installation
    docker ps
    docker version
    
    # Validate kind
    kind version
    
    # Validate kubectl
    kubectl version
    ```

# Configuring the Docker daemon

* if your Docker Daemon -- runs as a -- VM -> configure how much VM have
  * memory (>4 Gb of RAM)
  * CPUs, (>2)
  * disk space,
  * swap size

# Starting the Kubernetes cluster

```shell
kind create cluster
```

{% include quickstarts/create-commands.md %}

Enjoy your Apache Kafka cluster, running on Kind!

{% include quickstarts/delete-commands.md %}

{% include quickstarts/next-steps.md %}