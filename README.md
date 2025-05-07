### Apache-Airflow-Kubernetes
Apache Airflow is a platform created by the community to programmatically author, schedule and monitor workflows`https://airflow.apache.org/`
#### Principles
- Scalable: Apache Airflow has a modular architecture and uses a message queue to orchestrate an arbitrary number of workers. Airflow is ready to scale to infinity
- Dynamic: Apache Airflow pipelines are defined in Python, allowing for dynamic pipeline generation. This allows for writing code that instantiates pipelines dynamically
- Extensible: Easily define your own operators and extend libraries to fit the level of abstraction that suits your environment
- Elegant: Apache Airflow pipelines are lean and explicit. Parametrization is built into its core using the powerful Jinja templating engine