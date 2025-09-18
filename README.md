# Project Title: vincentkibet-sit722

## Getting Started

To get the project running, you can either set it up for local development using Docker Compose or deploy it to the cloud using the CI/CD pipeline.

## Prerequisites

* Git
* Docker & Docker Compose
* Azure CLI (for cloud deployment)
* A Microsoft Azure account (for cloud deployment)

## Local Development

Docker Compose is configured to orchestrate the entire application stack on a local machine.

1.  **Clone the Repository:**

    ```bash
    git clone [https://github.com/](https://github.com/)<your-username>/vincentkibet-sit722.git
    cd vincentkibet-sit722
    ```

2.  **Run the Application:**

    The `docker-compose.yml` file is located in the `frontend` directory.

    ```bash
    cd frontend
    docker-compose up --build
    ```

    This command builds the images for all services and starts the containers.

3.  **Access the Services:**
    * **Frontend:** [http://localhost:8080](http://localhost:8080)
    * **Product Service API:** [http://localhost:8000](http://localhost:8000)
    * **Order Service API:** [http://localhost:8001](http://localhost:8001)
    * **Customer Service API:** [http://localhost:8002](http://localhost:8002)
    * **RabbitMQ Management UI:** [http://localhost:15672](http://localhost:15672) (user: `guest`, pass: `guest`)

## CI/CD Pipeline

The core of this project is its automation pipeline, built using GitHub Actions. The pipeline is split into distinct workflows for staging and production environments, ensuring a safe path for code to reach users.

### Staging Workflow

* **Trigger:** A `git push` to the `testing` branch.
* **Actions:**
    * **Build & Push:** The pipeline builds Docker images for each microservice. Each image is tagged with both the unique Git commit SHA and the `:latest` tag.
    * **Deploy:** A new, temporary namespace is created in the AKS cluster (e.g., `staging-12345`). The Kubernetes manifests are then applied to this namespace, deploying the application using the `:latest` images.
    * **Manual Teardown:** A separate, manually triggered workflow (`destroy-staging.yml`) allows for the complete removal of the staging namespace and its resources after testing is complete.

### Production Workflow

* **Trigger:** A pull request being merged from the `testing` branch into the `main` branch.
* **Actions:**
    * **Prepare Manifests:** The workflow checks out the code corresponding to the merge commit. It then modifies the Kubernetes manifests, replacing the `:latest` image tag with the specific Git commit SHA. This step ensures that production deployments are immutable and traceable.
    * **Deploy:** The modified manifests are applied to the permanent `production` namespace in the AKS cluster, safely updating the live application.

## Author

* **Name:** Vincent Kibet