# 🚀 Python FastAPI CI/CD Pipeline with Jenkins & Docker

> A lightweight, production-ready Python FastAPI microservice featuring automated testing with pytest, containerization, robust health checking, and a fully automated CI/CD deployment pipeline powered by Jenkins and Docker.



<img width="1710" height="652" alt="Screenshot 2026-07-21 at 19 56 39" src="https://github.com/user-attachments/assets/bed17b79-fdbe-4cbc-9ae3-a8e3d6fd29d5" />


## 🛠️ Tech Stack & Tools

* **Backend:** Python 3.11, FastAPI, Uvicorn
* **Testing:** Pytest, HTTPX (TestClient)
* **Containerization:** Docker, Docker Compose
* **CI/CD Automation:** Jenkins (Declarative Pipeline)
* **Version Control:** Git & GitHub



## 📂 Project Structure

```text
├── main.py              # FastAPI application entry point
├── test_main.py         # Automated API tests using pytest
├── requirements.txt     # Python project dependencies
├── Dockerfile           # Docker image configuration for the API
└── jenkins/
    └── Jenkinsfile      # CI/CD pipeline definition
```



## 📋 Prerequisites & Required Plugins

To run and deploy this project locally using Jenkins and Docker, ensure you have the following installed and configured:

### 1. System Requirements

* **Docker & Docker Compose** installed on your host machine.
* **Linux/Debian-based environment** (recommended for Docker socket mapping).

### 2. Jenkins Setup & Plugins

Your Jenkins container must have access to the host's Docker daemon to build and deploy containers from the pipeline.

* **Docker-in-Docker / Docker-outside-of-Docker (DooD):** Configured via volume mounting (`/var/run/docker.sock`).
* **Jenkins Plugins:**
* **Git Plugin** (for repository checkout)
* **Pipeline Plugin** (for declarative pipeline support)
* **Docker Pipeline Plugin** (optional, if using agent-level docker blocks)

## 🚀 Getting Started Locally

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### 2. Run Jenkins via Docker Compose

Start your local Jenkins server with Docker socket access enabled:

```bash
docker compose up --build -d
```

Access Jenkins at `http://localhost:8080`.

### 3. Run the FastAPI App Locally (Optional)

If you want to run the application manually without Jenkins:

```bash
# Install dependencies
pip install -r requirements.txt

# Run the development server
uvicorn main:app --reload --port 8000
```

Open your browser and navigate to `http://localhost:8000/docs` to view the interactive Swagger documentation.

## 🔄 CI/CD Pipeline Workflow

The pipeline defined in the `Jenkinsfile` automates the entire software delivery lifecycle through the following stages:

1. **Checkout SCM:** Pulls the latest source code from the GitHub repository.
2. **Run Tests:** Executes automated `pytest` suites to validate application logic.
3. **Build Docker Image:** Packages the FastAPI application into a lightweight, optimized Docker container.
4. **Deploy Container:** Automatically stops any legacy container instances and boots up the fresh build on port `8000`.
5. **Health Check:** Executes an automated internal validation probe (`docker exec`) targeting the `/health` endpoint to guarantee the application initialised successfully and is ready to accept production traffic.
6. **Post Execution (always):** Automatically triggers a clean-up step (`docker image prune`) to keep the host disk free of dangling build cache images.
## 📸 Screenshots & Demo


### 1. Jenkins Pipeline Execution Success

<img width="1710" height="649" alt="Screenshot 2026-07-21 at 19 43 48" src="https://github.com/user-attachments/assets/e5a6d554-c1c7-440c-ba5b-dceef25ee848" />

### 2. FastAPI Interactive Swagger Documentation

<img width="1710" height="995" alt="Screenshot 2026-07-21 at 19 46 08" src="https://github.com/user-attachments/assets/21d66ef7-d2c4-4911-9f80-5ba1303b7083" />

### 3. Jenkins Health Check

<img width="1710" height="652" alt="Screenshot 2026-07-21 at 19 56 39" src="https://github.com/user-attachments/assets/bed17b79-fdbe-4cbc-9ae3-a8e3d6fd29d5" />

