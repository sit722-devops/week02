# Week 02 : Microservice Deployment with Docker

In Week 02, you will extend the Student Service developed in Week 01 by integrating a PostgreSQL database and containerising the application using Docker.

Unlike Week 01, where student information was stored in an in-memory data structure, this week's application stores student records in a PostgreSQL database using SQLAlchemy ORM.

You will also learn how to package the application into a Docker container, allowing it to run consistently across different environments.

The primary goal of this example is to cover fundamental **Docker** concepts: building an image and running a container.

Prerequisites

Before starting this practical, ensure you have the following installed:

1.  **Docker Desktop:**
    - Download from: [https://docs.docker.com/get-started/get-docker/](https://docs.docker.com/get-started/get-docker/)
2.  **Python 3.10+:**
    - Download from: [https://www.python.org/downloads/](https://www.python.org/downloads/)
3.  **PostgreSQL Database:**
    - You need a local PostgreSQL server instance.
    - **Recommended:** Install PostgreSQL directly on your machine (e.g., via Homebrew for macOS, apt for Linux, or a standalone installer for Windows). Download from [https://www.postgresql.org/download/](https://www.postgresql.org/download/)
    - **Alternative (using Docker for DB only):** If you prefer not to install PostgreSQL directly, you can run a PostgreSQL container temporarily:
      ```bash
      docker run --name local-postgres -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=students -p 5432:5432 -d postgres:15-alpine
      ```
      Remember to stop/remove it when done: `docker stop local-postgres && docker rm local-postgres`

4. Database Setup (Ignore this step if using Docker for DB)

The Student Service expects a PostgreSQL database named `students` with user postgres and password postgres.

- Start your local PostgreSQL server.

- **Create the students database:*** 
    Open your PostgreSQL client (like psql in your terminal or a GUI like pgAdmin) and run the following command:
    ```sql
    CREATE DATABASE students;
    ```

    (If you used the Docker command to run PostgreSQL locally, this database will be created automatically by the postgres:15-alpine image due to the POSTGRES_DB environment variable.)

## Setup Instructions

1. Clone the Repository

```bash
git clone https://github.com/sit722-devops/week02.git
```

2. Navigate to the Project Directory

```bash
cd week02
```
3. Open the Project in Visual Studio Code

Open the week02 folder using Visual Studio Code.

4. Create a Python Virtual Environment
```bash
# Create the virtual environment
python -m venv .venv

# Activate the virtual environment
# On macOS/Linux:
source ./.venv/bin/activate
# On Windows (Command Prompt):
# .\.venv\Scripts\activate.bat
# On Windows (PowerShell):
# .\.venv\Scripts\Activate.ps1
```

5. Install Dependencies:

With your virtual environment activated, install the required Python packages:

```bash
pip install -r requirements.txt
```

6. Run unit tests

Before running the application, execute the unit tests to verify that your changes have not introduced any issues.

```bash
pytest tests
```

Ensure that all tests pass successfully before proceeding to the next step.

7. Run Application Locally

- Start the FastAPI application.
```bash
uvicorn app.main:app --reload
```

When the application starts successfully, it will automatically create the required database table if it does not already exist.

Open your web browser and navigate to:

- Root Endpoint: [http://localhost:8000/](http://localhost:8000/docs)
- Swagger UI [http://localhost:8000/docs](http://localhost:8000/docs)

8. Build the Docker Image

From the project root directory, build the Docker image.
```bash
docker build -t student-service .
```

Run the Docker container using the following command.
```bash
docker run -p 8000:8000 \
-e POSTGRES_HOST=host.docker.internal \
-e POSTGRES_PORT=5432 \
-e POSTGRES_DB=students \
-e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=postgres \
student-service
```

**Note:** The value host.docker.internal allows the Docker container to connect to the PostgreSQL server running on your local machine. If PostgreSQL is running in another Docker container, replace this value with the name of that container

9. Verify the Docker Deployment

After the container starts successfully, verify that the application is running by opening:

http://localhost:8000/docs