# ELT with Airflow and dbt Core on Postgres

This is a modified version of the [Official Apache Airflow 2.2.2 Docker Compose](https://airflow.apache.org/docs/apache-airflow/2.2.2/start/docker.html) to include `pgAdmin` and `dbt Core 1.2.0` to allow the user to run the project's DAGs as well as query the Airflow Postgres database to inspect the data generated by the ELT process without relying on any other software installed in the machine.

This is not a production-ready container image and has many issues, such as unencrypted credentials and the use of Airflow's database and credentials by other applications. The goal of this project is to provide a starting point for those that want to learn more about dbt and Airflow.

## Prerequisites
This project requires only Docker to run the [ELT DAGs](dags) and [dbt project](dbt). Optionally, you can install `dbt Core 1.2.0` to serve the dbt documentation if you want to.

To install Docker, please refer to their [official website installation guide](https://docs.docker.com/get-docker/). You can look into system specific installation guides below:
- macOS: [Install Docker Desktop](https://docs.docker.com/desktop/install/mac-install/)
- Linux/Ubuntu: [Install Docker Desktop](https://docs.docker.com/desktop/install/linux-install/) or [Install Docker Engine](https://docs.docker.com/engine/install/)
- Windows: [Install Docker Desktop](https://docs.docker.com/desktop/install/windows-install/)

With Docker installed, you are ready to start!

If you want to serve the dbt documentation, make sure you have `python>=3.7` and `pip` installed in your system. Then, to install dbt Core you can follow the [official website installation guide](https://docs.getdbt.com/docs/core/pip-install) and install the Postgres adapter, or simply run the command below:
```
pip install dbt-postgres==1.2.0
```

## Get started
First clone the repository into your local machine:
```
git clone https://github.com/KehlRafael/airflow-dbt-imdb-elt.git
cd airflow-dbt-imdb-elt
```

### Running the containers
To run the containers specified in the [docker-compose.yml](docker-compose.yml) file you can simply run:
```
docker compose -p imdb_ratings up -d
```
There is no need to build the image beforehand. All containers will be listed under the `imdb_ratings` project.

### Accessing the UI
To access the Airflow UI you must:
- Open the Airflow UI at [http://localhost:8080/](http://localhost:8080/)
- Username: airflow
- Password: airflow

These are the default values in the [docker-compose.yml](docker-compose.yml). These values can be overwritten by adding to [.env](.env) the variables `_AIRFLOW_WWW_USER_USERNAME` and `_AIRFLOW_WWW_USER_PASSWORD`, they will set the username and password, respectively.

To access the `pgAdmin` UI you must:
- Open the `pgAdmin` UI at [http://localhost:8888/](http://localhost:8888/)
- Email: admin@admin.com
- Password: admin

These are the default values in the [docker-compose.yml](docker-compose.yml). These values can be overwritten by adding to [.env](.env) the variables `PGADMIN_DEFAULT_EMAIL` and `PGADMIN_DEFAULT_PASSWORD`, they will set the email and password, respectively.

In addition, if you have a preferred way to connect to and query databases, you can check the [pgpass](pgpass) and [servers.json](servers.json) files to get more information on the connection string needed to connect to the Postgres database used by this project.

### Serving dbt documentation
To serve dbt documentation, make sure you followed the optional steps in the [Prerequisites](#prerequisites) section.

Make sure both the [extract_and_load_datasets](./dags/extract_and_load_datasets.py) and [run_imdb_ratings_bash](./dags/run_imdb_ratings_dbt_bash.py) DAGs were already executed successfuly in Airflow. After that, run the following commands from the root of this project:
```
cd ./dbt/imdb_ratings
dbt docs serve --port 8001 --profiles-dir .
```
With that, documentation will be available at [http://localhost:8001/](http://localhost:8001/).

### Stopping and removing images
To stop the containers run:
```
docker compose -p imdb_ratings stop
```
To tear down the environment run:
```
docker compose -p imdb_ratings down
```
You can add the `-v` and `--rmi local` flags at the end to also remove volumes and images, respectively, that are associated with the project.

### Contact
If you have any questions, concerns or want to suggest any improvements, please get in touch through my contact infomation availabe at my GitHub profile, [kehlrafael](https://github.com/kehlrafael).

### License
See the [LICENSE](LICENSE) file.
