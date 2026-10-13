# Real-Time Operations Monitoring and Alerting

## Project Overview

This project implements a real-time operations monitoring and alerting system for **ZAPPTTO**, a delivery service. It simulates delivery metrics, collects and monitors them using Prometheus, visualizes performance through Grafana dashboards, and uses Jenkins for continuous integration and pipeline automation.

The main objective is to monitor delivery operations, identify potential performance issues, and generate alerts when predefined thresholds are exceeded.

## Objectives

- Simulate real-time delivery performance metrics using Python.
- Expose application metrics through an HTTP endpoint.
- Collect and monitor metrics using Prometheus.
- Visualize delivery statistics using Grafana dashboards.
- Configure alert rules for high pending deliveries and increased delivery time.
- Use Docker to run monitoring services in containers.
- Use Jenkins to validate project files through a CI pipeline.
- Store the source code and screenshots in GitHub.

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Simulates delivery metrics |
| Prometheus | Collects metrics and evaluates alert rules |
| Grafana | Visualizes metrics in dashboards |
| Docker | Runs services in containers |
| Docker Compose | Manages Prometheus and Grafana services |
| Jenkins | Automates project validation |
| Git and GitHub | Version control and project hosting |

## Project Structure

```text
delivery_monitoring/
├── delivery_metrics.py
├── requirements.txt
├── prometheus.yml
├── alert_rules.yml
├── Dockerfile
├── docker-compose.yml
├── Jenkinsfile
├── .gitignore
├── README.md
└── screenshots/
    ├── 01-python-output.png
    ├── 02-metrics-endpoint.png
    ├── 03-prometheus-targets.png
    ├── 04-prometheus-alerts.png
    ├── 05-grafana-datasource.png
    ├── 06-grafana-dashboard.png
    ├── 07-alert-firing.png
    └── 08-jenkins-success.png
```

*Note: The screenshot filenames above are examples. Include only files that you have actually captured.*

## Monitored Metrics

The Python application simulates the following metrics:

| Metric | Description |
|---|---|
| `total_deliveries` | Total number of deliveries in the current simulation |
| `pending_deliveries` | Number of deliveries waiting to be processed |
| `on_the_way_deliveries` | Number of deliveries currently on the way |
| `average_delivery_time_sum` | Cumulative sum of observed delivery times |
| `average_delivery_time_count` | Number of delivery-time observations |

The average observed delivery time is calculated using:

```promql
average_delivery_time_sum / average_delivery_time_count
```

The application generates simulated values periodically and exposes the metrics for Prometheus to scrape.

## Prerequisites

Install the following software before running the project:

- Python 3.10 or later
- Visual Studio Code
- Docker Desktop
- Git
- A GitHub account

Jenkins is required for the pipeline portion of the exercise.

## Installation and Setup

### 1. Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/delivery-monitoring.git
cd delivery-monitoring
```

Replace `YOUR-USERNAME` with your GitHub username.

### 2. Create and activate a Python virtual environment

On Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install -r requirements.txt
```

The `requirements.txt` file contains:

```text
prometheus-client
```

### 4. Start the Python application

```bash
python delivery_metrics.py
```

The application starts an HTTP server on port `8000` and periodically prints simulated delivery statistics.

Access the metrics endpoint at:

http://localhost:8000/metrics

Keep the application running while testing Prometheus and Grafana.

## Prometheus Configuration

Prometheus is configured through `prometheus.yml` to scrape:

- Its own metrics.
- The delivery application's metrics endpoint at `host.docker.internal:8000`.

The scrape interval is configured to five seconds.

Start Prometheus and Grafana with Docker Compose:

```bash
docker compose up -d
```

Check the running containers:

```bash
docker compose ps
```

Access the Prometheus interface:

- Prometheus: http://localhost:9090
- Targets: http://localhost:9090/targets
- Alerts: http://localhost:9090/alerts

The delivery application's target should display **UP** when the Python application is running and the connection is configured correctly.

## Grafana Dashboard

Grafana uses Prometheus as its data source to display delivery performance.

### Dashboard panels

| Panel | PromQL query |
|---|---|
| Total Deliveries | `total_deliveries` |
| Pending Deliveries | `pending_deliveries` |
| On-the-Way Deliveries | `on_the_way_deliveries` |
| Average Delivery Time | `average_delivery_time_sum / average_delivery_time_count` |

### Dashboard setup

1. Open http://localhost:3000.
2. Configure Prometheus as a data source.
3. When using the provided Docker Compose setup, set the Prometheus URL to `http://prometheus:9090`.
4. Save and test the data source connection.
5. Create a dashboard named **ZAPPTTO Delivery Monitoring**.
6. Add the four panels using the queries listed above.
7. Save the dashboard.

Grafana can display the metrics using Stat panels or time-series visualizations.

## Alert Configuration

The alert rules are defined in `alert_rules.yml`.

### 1. High Pending Deliveries

- **Alert name:** `HighPendingDeliveries`
- **Condition:** `pending_deliveries > 10`
- **Duration:** 15 seconds
- **Severity:** Warning

This alert identifies situations where pending deliveries remain above the configured threshold.

### 2. High Average Delivery Time

- **Alert name:** `HighAverageDeliveryTime`
- **Condition:** Average observed delivery time is greater than 30 seconds.
- **Duration:** 15 seconds
- **Severity:** Critical

This alert identifies potential delays in delivery operations.

Prometheus evaluates these rules and displays their states on its Alerts page. Configuring email, SMS, or other external notifications requires an additional notification receiver.

## Docker Image

The `Dockerfile` packages the Python application into a Docker image.

Build the image:

```bash
docker build -t delivery-metrics .
```

The image exposes port `8000` for application metrics.

Prometheus and Grafana are managed separately through Docker Compose.

## Jenkins Pipeline

The `Jenkinsfile` defines a continuous integration pipeline with the following stages:

1. **Checkout:** Retrieves the project from source control.
2. **Validate Project Files:** Checks that the required project files exist.
3. **Validate Dockerfile:** Checks that the Dockerfile contains a Python base image declaration.

The pipeline reports success when the validation stages complete successfully.

The current Jenkins pipeline validates the project files; it does not automatically deploy the entire monitoring stack.

## Screenshots and Results

The `screenshots/` directory contains evidence captured while executing the project.

Recommended screenshots:

1. Python application output.
2. Metrics endpoint in the browser.
3. Prometheus targets showing the delivery service as UP.
4. Prometheus alerts page.
5. Successful Grafana data source connection.
6. Grafana dashboard with four metric panels.
7. A firing alert, if observed.
8. Successful Jenkins pipeline execution.

### Results

The project demonstrates how simulated operational metrics can be exposed, collected, visualized, and monitored using a combination of Python, Prometheus, Grafana, Docker, and Jenkins.

The system provides a foundation for identifying high pending-delivery counts and increased delivery times through configurable monitoring rules.

## Future Enhancements

- Integrate real delivery-service data instead of simulated values.
- Configure Alertmanager for email or other notifications.
- Add automated application tests to the Jenkins pipeline.
- Automate Docker image builds and deployment through Jenkins.
- Add more metrics, dashboards, and service-health checks.
- Improve dashboard design with thresholds and service-level indicators.

## Conclusion

This project demonstrates a basic DevOps monitoring workflow for a delivery service. Python generates sample operational metrics, Prometheus collects and evaluates them, Grafana presents the results, Docker runs the supporting services, and Jenkins validates the project through a CI pipeline.

The implementation provides practical experience with application monitoring, metrics collection, visualization, alert rules, containerization, and version control.

## Author

**Name:** YOUR-NAME

**GitHub Repository:** https://github.com/YOUR-USERNAME/delivery-monitoring
