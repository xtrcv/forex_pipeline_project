# forex_pipeline_project (notes for myself)


## Import

The dag begins by importing Airflow and all of the operators needed, as well as csv, json, and request modules. 

## default_args

The default_args dictionary defines default parameters that will be used when running the dag. For example, e-mail can be defined to inform the owner whether dag suceeds or fails. 

## Download_rates() Function
Download_rates() function will download forex rates from api.exchangeratesapi.io. Unfortunately the API doesn't work at the moment as the website is under maintenance at the time of writing (2023-01-17).

The function opens forex_currencies.csv and download the pairs of currencies that are described in the .csv

## forex_data_pipeline_final DAG
forex_data_pipeline_final dag instantiated and default_args are passed to it. schedule_interval is defined, and is quite important, because when the DAG is being run, Airflow will wait the schedule_interval amount of time before running the DAG.

Two sensors - HttpSensor and FileSensor are used to check if the HTTP endpoint is available and forex rates .csv file has arrived in the defined directory. 

The bash operator then runs a couple of commands to create a directory and to place the .json file in it.

## Hive
To interact with Hive, a connection has to be established by going to the Airflow UI (localhost:8080/home via borowser), Admin -> Connections -> Plus sign to add a new record -> Type in hive_conn as conn id -> Set Hive Server 2 Thrift as Conn Type -> Set host as hive-server -> Type in "hive" as both password and login (use something much more difficult in prod/dev) -> Set port as 10000, although I believe this can be modified if needed. Save and that's it, connection established. 

To test the connection, go the the Docker container of Airflow by executing "docker exec -t [airflow_container_id] /bin/bash]" and type "airflow tasks test [dag_id] [task_id] [execution_date_in_the_past]". If everything is OK and there is "Marking task as SUCCESS" message then everything is working well! Refresh the the table on 

A hive table is created so that I could interact with data via SQL-like queries (hql). This particular hql will create a table with columns that will correspond to the data in forex_rates.json 

Spark operator runs as decribed in the airflow/dags/scripts/forex_processing.py script.

If the DAG has run successfully until this stage, it will send an e-mail. Slack API operator can also be used to send a message on it.
