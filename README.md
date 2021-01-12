# Airflow Using Docker or Kubernetes
    Deploy airflow with docker compose and kubernetes as well.
    Using PythonOperator, KubernetesPodOperator and KubernetesExecutor as well

### Prerequisites
+ Docker & Docker Compose
+ Minkube on local
+ Postgres 12
+ Airflow 1.10.12

### Structure folders
```
|-airflow-k8s-pod-operator: run and deploy KubernetesPodOperator
|-airflow-k8sexecutor: run and deploy Kubernetes Executer
|-k8s: deploy on Kubernetes
|-scripts: using docker-compose
|-dags: dag files using docker-compose
|-docker-compose
|-Dockerfile
```

### Docker Compose
+ Create a file .env
    ```
    AIRFLOW__CORE__LOAD_DEFAULT_CONNECTIONS=False
    AIRFLOW__CORE__SQL_ALCHEMY_CONN=postgres+psycopg2://airflow:airflow@postgres:5432/airflow
    AIRFLOW__CORE__FERNET_KEY=81HqDtbqAywKSOumSha3BhWNOdQ26slT6K0YaZeZyPs=
    AIRFLOW_CONN_METADATA_DB=postgres+psycopg2://airflow:airflow@postgres:5432/airflow
    AIRFLOW_VAR__METADATA_DB_SCHEMA=airflow
    AIRFLOW__SCHEDULER__SCHEDULER_HEARTBEAT_SEC=10
    ```

+ Run webserver & scheduler
    ```
    docker-compose up
    ```

### Kubernetes
+ Build and Push Image to Docker Hub(airflow-k8sexecutor)
    ```
    docker build --tag=[image name] .
    docker push [image name]
    ```

+ Deploy config map
    ```
    kubectl create -f k8s/airflow-configmap.yml
    ```

+ Deploy Postgres database
    ```
    kubectl create -f k8s/postgres/deployment.yml
    kubectl create -f k8s/postgres/service.yml
    ```

+ Deploy Scheduler
    ```
    kubectl create -f k8s/scheduler.yml
    ```

+ Deploy WebServer
    ```
    kubectl create -f k8s/webserver.yml
    ```

+ Connect Airflow WebServer UI
    ```
    kubectl port-forward service/airflow-webserver 8080:8080
    ```

+ Open http://localhost:8080/admin in your browser


### Reference
+ [Three ways to run airflow on Kubernetes](https://fullstaq.com/blog/three-ways-to-run-airflow-on-kubernetes/)
+ [Airflow with docker](https://blog.knoldus.com/running-apache-airflow-dag-with-docker/)
+ [Airflow Kubernetes](https://github.com/multilayer-io/airflow-kubernetes)