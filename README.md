# EXCHANGE RATE PROJECT - URUGUAY

The purpose of this document is to serve as a guide for those interested in understanding the aim and scope of the project. It will provide a detailed explanation of each component, its function, and the rationale behind its utilization. As the document progresses, diagrams will be included to aid comprehension of the concepts discussed.

## Project Objective:
The project aims to gather information on the buying and selling exchange rates for various currencies across different institutions. This information is extracted from the official website of each institution at predefined intervals (every 30 minutes).

Below is a table showcasing the institutions and quotations that are currently registered.


| Currency           | Aspen | Bacacay | Brou  | Cambilex | Gales | Misiones | Obelisco | Varlix |
|--------------------|-------|---------|-------|----------|-------|----------|----------|--------|
| CIERRE BILLETE USA | -     | YES     | -     | -        | -     | -        | -        | -      |
| Dolar USA          | YES   | YES     | YES   | YES      | YES   | YES      | YES      | YES    |
| DOLAR USA EBROU    | -     | -       | YES   | -        | -     | -        | -        | -      |
| EURO               | YES   | YES     | YES   | YES      | YES   | YES      | YES      | YES    |
| FRANCO SUIZO       | -     | -       | YES   | -        | -     | -        | -        | -      |
| GUARANÍ            | -     | -       | YES   | -        | -     | -        | -        | -      |
| LIBRA ESTERLINA    | -     | -       | YES   | -        | -     | -        | -        | -      |
| PESO ARGENTINO     | YES   | YES     | YES   | YES      | YES   | YES      | YES      | YES    |
| PESO CHILENO       | -     | YES     | -     | -        | -     | -        | -        | -      |
| REAL BRASILEÑO     | YES   | YES     | YES   | YES      | YES   | YES      | YES      | YES    |
| UNIDAD INDEXADA    | -     | -       | YES   | -        | -     | -        | -        | -      |
| ONZA TROY DE ORO   | -     | -       | YES   | -        | -     | -        | -        | -      |

The project is purely educational in nature, aiming to orchestrate various technologies to create a functional end-to-end solution. The process encompasses extraction, transformation, loading, and culminates in data visualization.

## Data Ingestion Process:
The initial step of the process involves data ingestion. Web scraping techniques (utilizing the Python language) are employed for this purpose, enabling the extraction of relevant information (currencies and exchange rates) by parsing the structure of each website. Since institutions maintain separate websites, a file is generated for each site in order to facilitate subsequent processing and code maintenance.

## Transformation Process:
Upon data retrieval, the transformation process ensues. This phase is responsible for cleaning the data before it is entered into the productive repository, from which visualizations will be generated.

Specifically, cleaning transformations are performed, where accents are removed, and currency codes are standardized. Simultaneously, "administrative" columns such as execution time and data source are added. Once the data is cleaned, it is stored in the productive database.

It's important to note that all these transformations are carried out in Python.

## Storage Process:
In the storage phase, emphasis is placed on two initial instances. The first instance involves storing raw data.

Once the data is retrieved, it is immediately stored temporarily in a MongoDB database. This database serves as a backup in case the processing script encounters an error that prevents the productive data loading.

The second instance involves an InfluxDB database, from which the necessary queries will be made and the data will be visualized subsequently.

## Architecture Diagram:
![Architecture](TCU-ARCHITECTURE.svg)

## Technologies Used and Justification:
Among the technologies employed to build the solution, the following stand out:

**DigitalOcean Server**: This server hosts the entirety of the project.

**Git & GitHub**: Version control used for development and deployment. It serves as a means to transfer local files to the server.

**Docker**: Containerization technology serving as the foundation of the entire project. It's used to deploy all services developed thereafter. Docker is chosen because it simplifies the process of moving code from the local environment to the production environment without encountering dependency or versioning issues. Simultaneously, it allows for quick service updates.

**MongoDB**: NoSQL database utilized as temporary storage. Raw data is stored as a backup in case processing fails. Once successful loading into the productive database is confirmed, these records are removed. The decision not to retain originals is based on storage concerns. MongoDB is chosen because it's ideal for storing semi-structured records and very useful where filtering by values is required.

**Mongo-Express**: A tool used to easily access records (if they exist) stored in MongoDB. While not necessary, it facilitates interaction with the database. The downside of this convenience is introducing a vulnerability to the system since it requires exposing a port externally. However, since public data is being handled, it's not a major issue for the development of this project.

**InfluxDB**: A NoSQL database oriented towards time series data. This database was chosen because it's specifically designed to handle time series data, maximizing efficiency when generating queries and aggregations that require a temporal filter.

**RabbitMQ**: A message queue system. It's utilized in the development code to facilitate the transmission of raw data to MongoDB, processed data to InfluxDB, and error notifications. It enables asynchronous data processing and decoupling. This means that it's not necessary to wait for all records to be successfully written to MongoDB before continuing with processing.

While this component isn't strictly necessary (due to the low volume of records being handled), it's used for the educational value it provides.

**Grafana**: A web visualization tool used to present various relevant graphs to users.

**Telegraf**: A server agent responsible for collecting metrics from the state of containers and the DigitalOcean server. These metrics are used to determine if any configuration modifications to container resources are necessary or if system scaling is required.

Together, Grafana, InfluxDB, and Telegraf form what is known as the TIG stack.

**Nginx**: Utilized for its capabilities as a web server and reverse proxy. It efficiently and securely serves the solution.

**Certbot**: A tool used to generate SSL certificates that enable secure HTTPS connections.

**Airflow**: A process orchestrator used to schedule, execute, and remove containers and metadata generated by the process (to manage storage).

## Status on 2024-06-09 (YYYY-MM-DD):
#### Frontend (Ngnix + Cerbot)
![main_webpage](main.png)
