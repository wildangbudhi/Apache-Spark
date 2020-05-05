# Apache-Spark

Here we are going to setup and try Apache Spark to process masive data or big data

## 2 Worker and 1 CPU Cores

### STEP 1 : Create Apache Spark Cluster

In this step we are going to create Apache Spart Cluster contains 1 Master Node followed with 2 Worker that alocated to use 2 CPU Cores each. To create those 3 Node wa are going to use Docker Container so we can make 3 Different Node that decoupled each environtment. To ease creating Docker Container we will use ```docker-compose```.

1. Create a directory for working directory. In this case i will make a directory called ApacheSpark and change directory to those directory

2. We need to create docker-compose configuration to create and run Docker Container. We can download the template from https://raw.githubusercontent.com/bitnami/bitnami-docker-spark/master/docker-compose.yml .

    ```yml
    version: '2'

    services:
    spark:
        image: bitnami/spark:2
        environment:
        - SPARK_MODE=master
        - SPARK_RPC_AUTHENTICATION_ENABLED=no
        - SPARK_RPC_ENCRYPTION_ENABLED=no
        - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
        - SPARK_SSL_ENABLED=no
        ports:
        - '8080:8080'
    spark-worker-1:
        image: bitnami/spark:2
        environment:
        - SPARK_MODE=worker
        - SPARK_MASTER_URL=spark://spark:7077
        - SPARK_WORKER_MEMORY=1G
        - SPARK_WORKER_CORES=1
        - SPARK_RPC_AUTHENTICATION_ENABLED=no
        - SPARK_RPC_ENCRYPTION_ENABLED=no
        - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
        - SPARK_SSL_ENABLED=no
    spark-worker-2:
        image: bitnami/spark:2
        environment:
        - SPARK_MODE=worker
        - SPARK_MASTER_URL=spark://spark:7077
        - SPARK_WORKER_MEMORY=1G
        - SPARK_WORKER_CORES=1
        - SPARK_RPC_AUTHENTICATION_ENABLED=no
        - SPARK_RPC_ENCRYPTION_ENABLED=no
        - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
        - SPARK_SSL_ENABLED=no
    ```

3. Create the Docker Container an run

    ![docker-compose up](https://github.com/wildangbudhi/Apache-Spark/blob/master/Screenshoot/docker-compose.png)

    To create and run Docker Container using ```docker-compose``` we can use command bellow:

    ```bash
    docker-compose up -d
    ```

    Parameters:
    - ```up``` : to create all container config inside docker-compose.yml and run it
    - ```-d``` : to run the container in deamon mode or background proess.

4. Test Apache Spark using UI Dashboard

    ![Test Apache Spark](https://github.com/wildangbudhi/Apache-Spark/blob/master/Screenshoot/Test%20Apache%20Spark.png)

    We can access Apache Spark dashboard through http://localhost:8080

5. Find Docker Container ID of Master Node

    We need Docker Container ID of Mater Node to Submit Job that we want to process later. We can use command bellow and find coresponding Container.

    ```bash
    docker container ls -a
    ```

6. Exec the Master Node

    ![Exec Master Node](https://github.com/wildangbudhi/Apache-Spark/blob/master/Screenshoot/docker-compose.png)

    We are going to akses the container image. This process is similiar to SSH Connaction to Server or Virtual Machine, but in this case we are going to access Container. We can use command below :

    ```bash
    docker exec -it <container_id> /bin/bash
    ```

    Parameters:
    - ```-it``` : to make this exec process not only run one command, so we can do all ubuntu command
    - ```/bin/bash``` : what base command that we want to use

7. Get Master Node Container Hostname

    We need Master Node Container Hostname to use it on Submiting Job later. We can use command below:

    ```bash
    hostname -i
    ```

8. Submit Job with 10 Partition


