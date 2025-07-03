# End-To-End-ETL-Pipeline-Using-Airflow-And-Astro

This project implements an **end-to-end ETL (Extract, Transform, Load) pipeline** for weather data using [Apache Airflow](https://airflow.apache.org/) and [Astro (Astronomer)](https://www.astronomer.io/). The pipeline fetches weather data from an open API, processes it, and loads it into a PostgreSQL database. The project structure, architecture, and tooling closely follow the tutorial from [this YouTube video](https://www.youtube.com/watch?v=Y_vQyMljDsE)[3].

---

## Table of Contents

- [Project Structure](#project-structure)
- [Architecture](#architecture)
- [Tools Used](#tools-used)
- [Pipeline Overview](#pipeline-overview)
- [Setup & Installation](#setup--installation)
- [Usage](#usage)
- [Screenshots](#screenshots)
- [References](#references)

---

## Project Structure

The project follows a standard Airflow setup initialized via Astro CLI:

- **dags/**: Contains all DAG scripts. `etl_weather.py` defines the ETL workflow.
- **Dockerfile**: Builds the Airflow environment.
- **requirements.txt**: Lists required Python packages.
- **airflow_settings.yaml**: Airflow connections and variables.
- **docker-compose.yaml**: Defines services (Airflow, PostgreSQL) for local development.

---

## Architecture

The pipeline is built as a **Directed Acyclic Graph (DAG)** in Airflow, consisting of three main tasks executed sequentially:

| Step                     | Task Name                | Description                                                      |
|--------------------------|--------------------------|------------------------------------------------------------------|
| Extract                  | `extract_weather_data`   | Fetches weather data from the Open-Meteo API using HTTP Hook.    |
| Transform                | `transform_weather_data` | Processes and cleans the extracted data.                         |
| Load                     | `load_weather_data`      | Loads the transformed data into a PostgreSQL database.           |

**Workflow Diagram:**


- Each task is implemented using Airflow's `@task` decorator.
- Data is passed between tasks using Airflow's XCom mechanism.
- The pipeline is scheduled to run **daily** (customizable).

---

## Tools Used

- **Apache Airflow**: Workflow orchestration and scheduling.
- **Astro (Astronomer)**: Manages Airflow deployments and simplifies development.
- **Docker**: Containerizes Airflow and PostgreSQL for consistent environments.
- **PostgreSQL**: Stores the processed weather data.
- **Open-Meteo API**: Provides free weather data for extraction.
- **Python**: Core language for DAG and task implementation.
- **Airflow Providers**: HTTP and Postgres hooks for external integrations.

---

## Pipeline Overview

1. **Extract**
   - Uses Airflow's `HttpHook` to call the Open-Meteo API.
   - Fetches current weather data for a specified location (latitude/longitude).

2. **Transform**
   - Parses the JSON response.
   - Selects and formats relevant fields (temperature, wind speed, etc.).

3. **Load**
   - Uses Airflow's `PostgresHook` to connect to the database.
   - Creates the target table if it doesn't exist.
   - Inserts the transformed data.

---

## Setup & Installation

### Prerequisites

- **Docker** (with Docker Compose)
- **Astro CLI**
- **Python 3.8+**

### Steps

1. **Install Astro CLI**

2. **Initialize the Project**

3. **Configure Connections**
- **Postgres Connection**: Set up in Airflow UI (`postgres_default`).
- **API Connection**: Set up in Airflow UI (`open_meteo_api`).

4. **Start Services**

This will launch Airflow and PostgreSQL containers.

5. **Access Airflow UI**
- Default: [http://localhost:8080](http://localhost:8080)
- Login: `admin` / `admin`

6. **Trigger the DAG**
- Enable and trigger `weather_etl_pipeline` from the Airflow UI.

---

## Usage

- **Scheduling**: The pipeline is set to run daily by default.
- **Manual Run**: Trigger runs from the Airflow UI as needed.
- **Monitoring**: Use Airflow's UI to monitor task status, logs, and data flow.

---

## Screenshots

**DAG Graph View:**

> ![DAG Graph View](attachment:image.jpg)[1]

**Airflow DAGs List:**

> ![Airflow DAGs List](attachment:Screenshot-2025-07-02-210637.jpg)[2]

---

## References

- [YouTube Tutorial: Apache Airflow One Shot- Building End To End ETL Pipeline Using AirFlow And Astro](https://www.youtube.com/watch?v=Y_vQyMljDsE)[3]
- [Official Astronomer Documentation](https://docs.astronomer.io/)
- [Open-Meteo API Documentation](https://open-meteo.com/)

---

## License

MIT License

---

**Author:**  
Adapted and implemented following [Krish Naik's tutorial](https://www.youtube.com/watch?v=Y_vQyMljDsE)[3].

