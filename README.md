### Apache-Airflow-Kubernetes
Apache Airflow is a platform created by the community to programmatically author, schedule and monitor workflows`https://airflow.apache.org/`
#### Principles
- Scalable: Apache Airflow has a modular architecture and uses a message queue to orchestrate an arbitrary number of workers. Airflow is ready to scale to infinity
- Dynamic: Apache Airflow pipelines are defined in Python, allowing for dynamic pipeline generation. This allows for writing code that instantiates pipelines dynamically
- Extensible: Easily define your own operators and extend libraries to fit the level of abstraction that suits your environment
- Elegant: Apache Airflow pipelines are lean and explicit. Parametrization is built into its core using the powerful Jinja templating engine
#### Datasets
Datasets are logical groupings of data: `pointer to some resource within Airflow`
- Airflow datasets are used for scheduling DAGS
#### Xcom
Xcoms: `Cross-Communication` are a mechanism that let Tasks talk to each other, as by default Tasks are entirely isolated and may be running on entirely different machines.
- Sharing data between tasks
#### Trigger Rules
- `all_success:(default)` The task runs only when all upstream tasks have succeeded
- `all_failed`: The task runs only when all upstream tasks are in a failed or upstream_failed state
- `all_done`: The task runs once all upstream tasks are done with their execution
- `all_skipped`: The task runs only when all upstream tasks have been skipped
- `one_failed`: The task runs only when at least one upstream task has failed
- `one_success`: The task runs when at least one upstream task has succeeded
- `one_done`: The task runs when at least one upstream task has either succeeded or failed
- `none_failed`: The task runs only when all upstream tasks have succeeded or been skipped
- `none_failed_min_one_success`: The task runs only when all upstream tasks have not failed or upstream_failed and at least one upstream task has succeeded
- `none_skipped`: The task runs only when no upstream task is in a skipped state
- `Always`: The task runs at anytime
### Connection to AWS
- Create a bucket ,bucket policy and attach user permission to access the AWS Console  
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Action": [
                "s3:ReplicateObject",
                "s3:PutObject",
                "s3:GetObject",
                "s3:RestoreObject",
                "s3:ListBucket",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::apache-airflow-backend/*",
                "arn:aws:s3:::apache-airflow-backend"
            ]
        }
    ]
}
```
- Create a connection on the Airflow Dashboard `a.k.a` -> `Connection ID`, `Connection Type`, `AWS Access Key ID` and `AWS Secret Access Key`
- Then add this Configs to your `.env` -> `AIRFLOW__CORE__XCOM__BACKEND="airflow.providers.common.io.xcom.backend.XComObjectStorageBackend" ` + `AIRFLOW__COMMON_IO__XCOM_OBJECTSTORAGE_PATH="S3://my_aws_conn@apache-airflow-backend/xcom"` + `AIRFLOW__COMMON_IO__XCOM_OBJECTSTORAGE_THRESHOLD="0"`
- Restart the development environment `astro dev restart` and `astro dev run dags test extractor 2025-01-01`
- `NOTE:` To use the local airflow storage -> change the threshold to `-1` in the environmental variables `AIRFLOW__COMMON_IO__XCOM_OBJECTSTORAGE_THRESHOLD="-1"` 
#### Variables
- Create a variable for the API from the Astro CLI `astro run variables set api "https://www.thecocktaildb.com/api/json/v1/1/random.php"`
- Check for created variable `astro dev run variables get api `
- Retrieve the `api` in the `task file` -> within the `_get_cocktail()`
```
from airflow.models import Variable

api = Variable.get('api')
```
