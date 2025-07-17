# Week 02 : Microservice Deployment with Podman

This example demonstrates a single **Product Microservice** built with FastAPI and PostgreSQL, running in a Podman container. The primary goal of this example is to cover fundamental **Podman** concepts: building an image and running a container.

1.  **Podman Desktop:**
    - Download from: [https://podman-desktop.io](https://podman-desktop.io)
2.  **Python 3.10+:**
    - Download from: [https://www.python.org/downloads/](https://www.python.org/downloads/)
3.  **PostgreSQL Database:**
    - You need a local PostgreSQL server instance.
    - **Recommended:** Install PostgreSQL directly on your machine (e.g., via Homebrew for macOS, apt for Linux, or a standalone installer for Windows). Download from [https://www.postgresql.org/download/](https://www.postgresql.org/download/)
    - **Alternative (using Podman for DB only):** If you prefer not to install PostgreSQL directly, you can run a PostgreSQL container temporarily:
      ```bash
      podman run --name local-postgres -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=products -p 5432:5432 -d postgres:15-alpine
      ```
      Remember to stop/remove it when done: `podman stop local-postgres && podman rm local-postgres`
4. Database Setup (Ignore thisstep )

    The Product Service expects a PostgreSQL database named `products` with user `postgres` and password `postgres`.

    - Start your local PostgreSQL server.

    - **Create the `products` database:**
        Open your PostgreSQL client (like `psql` in your terminal or a GUI like pgAdmin) and run the following command:
        ```sql
        CREATE DATABASE products;
        ```
        (If you used the Podman command to run PostgreSQL locally, this database will be created automatically by the `postgres:15-alpine` image due to the `POSTGRES_DB` environment variable.)
5.  Running the Product Service (Using Podman)
