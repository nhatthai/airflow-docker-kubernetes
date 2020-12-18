# Airflow K8S Executor
The KubernetesExecutor is an abstraction layer that enables any task in your DAG to be run as a pod on your Kubernetes infrastructure. You configure this executor as part of your Airflow Deployment just like you would any other executor, albeit some additional configuration options are required.

The KubernetesExecutor is great because you get the native elasticity of kubernetes together with all the good stuff from Airflow. It is no longer necessary to build your own containers with custom workloads as any task in your DAG is going to be run as a pod. There is some additional initial configuration required when setting up Airflow but the setup remains fairly easy to manage.

Do take into account that when you are going to be scheduling lots of tasks in parallel this could become very expensive for your cluster resources very quickly. Always keep an eye on this or set limits by using Airflow Pools (They can be set via ENV vars or via the UI).

Also, there is some startup and shutdown overhead every time a task gets spin up as a Pod. But depending on your expectations or requirements this is negligible.

+ Build image
    ```
    docker build .
    ```

+ Tag image
    ```
    docker tag [tag id]
    ```

+ Push image to Docker Hub
    ```
    docker push [tag id]
    ```

+ Deploy Kubernetes Executor
    ```
    the same deploy in the [README](./../README.md) file
    ```

+ Turn on the task and check Kubernetes pod
    ```
    kubectl get pods
    ```