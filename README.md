# airflow-docker-kubernetes
Deploy airflow with docker compose and kubernetes as well.


### Prerequisites
+ Docker
+ Minkube on local
+ Postgres 12
+ Airflow 1.10.12


### Structure folders
+ airflow-k8s-pod-operator: run KubernetesPodOperator
+ airflow-k8sexecutor: run Kubernetes Executer
+ scripts: using docker-compose
+ dags: dag files using docker-compose


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
    docker build .
    docker tag [image name]
    docker push [image name]
    ```

+ Deploy config map
    ```
    kubectl create -f airflow-k8s/airflow-configmap.yml
    ```

+ Deploy Postgres database
    ```
    kubectl create -f airflow-k8s/postgres/deployment.yml
    kubectl create -f airflow-k8s/postgres/service.yml
    ```

+ Deploy Scheduler
    ```
    kubectl create -f airflow-k8s/scheduler.yml
    ```

+ Deploy WebServer
    ```
    kubectl create -f airflow-k8s/webserver.yml
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