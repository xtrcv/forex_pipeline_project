# forex_pipeline_project (notes for myself)


## Import

The dag begins by importing Airflow and all of the operators needed, as well as csv, json, and request modules. 

## default_args

The default_args dictionary defines default parameters that will be used when running the dag. For example, e-mail can be defined to inform the owner whether dag suceeds or fails. 

## Download_rates() Function
Download_rates() function will download forex rates from api.exchangeratesapi.io. Unfortunately the API doesn't work at the moment as the website is under maintenance at the time of writing (2023-01-17).

## forex_data_pipeline_final DAG
forex_data_pipeline_final dag instantiated and default_args are passed to it. schedule_interval is defined, and is quite important, because when the DAG is being run, Airflow will wait the schedule_interval amount of time before running the DAG.

Two sensors - HttpSensor and FileSensor are used to check if the HTTP endpoint is available and forex rates .csv file has arrived in the defined directory.

