# Airflow and Spark: Running Spark jobs on Airflow (Docker-based solution)

In this repository, we've developed a Docker-based environment tailored for executing scheduled Spark jobs via Airflow. This approach addresses various development-related challenges, such as environmental discrepancies, missing dependencies, portability, and compactness. Consequently, this accelerates development and facilitates seamless deployment to production.

Several merits accompany the adoption of Docker containers to execute Spark jobs:

Enhanced Collaboration: Dockerization allows hassle-free sharing of development environments with colleagues, ensuring that all necessary dependencies are bundled.

Platform Agnostic Deployment: Docker enables you to construct and deploy your solutions across diverse cloud platforms like AWS, Azure, GCP, ensuring consistent outcomes.

Optimal Isolation: Docker empowers you to package distinct applications on the same machine—such as Spark, Kafka, and Airflow—ensuring they work harmoniously despite varying dependencies. This isolation safeguards against compatibility issues.

Resource Efficiency: By leveraging Docker containers, you can manage resource allocation more efficiently, aggregating RAM and CPU according to the demands of different applications.

To effectively utilize this environment, follow these steps:

*  Clone the Github repository 
*  Build the Spark and the Airflow image
*  Create your dags, logs, plugins folder
*  Create your environment variable
*  Start and run the Spark and Airflow containers 
*  Run your Spark jobs to confirm if the Spark job completed successfully before moving it to Airflow 
*  Design the Airflow DAG to trigger and schedule the Spark jobs.

## Clone the Github repository.
```bash
git clone https://github.com/yTek01/docker-spark-airflow.git
```

## Build the Spark image.
```bash
docker build -f Dockerfile.Spark . -t spark-air
```

## Build the Airflow image.
```bash
docker build -f Dockerfile.Airflow . -t airflow-spark
```

## Create your dags, logs, plugins folder.
```bash
mkdir ./dags ./logs ./plugins
echo -e "AIRFLOW_UID=$(id -u)\nAIRFLOW_GID=0" > .env
```

## Your environment variable would look like this.
```bash
AIRFLOW_UID=33333
AIRFLOW_GID=0
AWS_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXX
AWS_SECRET_KEY=XXXXXXXXXXXXXXXXXXXX
```

## Start and run the Spark and Airflow containers.
```bash
docker-compose -f docker-compose.Spark.yaml -f docker-compose.Airflow.yaml up -d
```
When all the services all started successfully, now go to http://localhost:8080/ to check that Airflow has started successfully, and http://localhost:8090/ that Spark is up and running. 


* Run your Spark jobs to confirm if the Spark job completed successfully before moving it to Airflow.

```bash
docker exec -it <Spark-Worker-Contianer-name> \
    spark-submit --master spark://XXXXXXXXXXXXXX:7077 \
    spark_etl_script_docker.py
```

If all is fine with the setup, i.e. the Spark job completed successfully, then move forward to scheduling the Spark job on Airflow. 

## Requirement.txt files (How to add missing libraries)
We've included two essential files, namely "requirements_airflow.txt" and "requirements_spark.txt," within our setup. These files list the necessary Python libraries and Airflow providers required for seamless operation. If you find yourself needing additional libraries or specific Airflow providers that are not currently listed, the solution is straightforward. You just need to include the desired library or provider in the corresponding "requirements.txt" file. Afterward, you can proceed to build and execute the container. This approach ensures that your environment remains tailored to your evolving needs, all while maintaining the simplicity and efficiency of the process.
