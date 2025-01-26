# Django Microservices with Docker and Nginx

This repository demonstrates a Django microservices architecture using Docker, Nginx, and PostgreSQL. Each service is isolated, and the infrastructure is orchestrated with Docker Compose. Nginx acts as the reverse proxy to route traffic to respective services.


## Features
- **Dockerized Architecture**: Leverages Docker Compose for service orchestration.
- **Microservices**: Separate Django applications for modular development.
- **PostgreSQL**: A robust relational database.
- **Nginx**: Acts as a reverse proxy and serves static/media files.
- **Custom Django Base Image**: Utilizes a custom Django base image hosted on Docker Hub.
- **Environment-Specific Boot Script**: Services adapt to either development or production mode based on environment variables.
- **Makefile**: Simplifies common operations such as building, starting, and stopping services.

## Prerequisites
- **Docker** and **Docker Compose** installed.
- if you want using **Makefile** make sure `make` package installed on your os. ([windows](https://stackoverflow.com/questions/32127524/how-to-install-and-use-make-in-windows), [Linux](https://askubuntu.com/questions/161104/how-do-i-install-make))
- Access to the [custom Django base image](https://hub.docker.com/r/imansadatii/python-base). Ensure the image is pulled locally before building the services.


## Services
Consider these aliases (service_1, service_2) to make it easier to replace your own name. Be sure to replace these names with your own name wherever you see them.
### Service 1

- **Build Context**: `./services/service_1`
- **Port**: 8001 on locally
- **Dockerfile**:
  with some specific settings and runs `check_postgres.sh` & `start.sh` script. 

### Service 2

- **Build Context**: `./services/service_2`
- **Port**: 8002 on locally
- **Dockerfile**:
  Similar to `service_1`, with its own app-specific logic.

### Nginx

- **Config**: Located at `./services/nginx/nginx.conf`
- **Routes**:
  - `/service_1/` -> Service 1
  - `/service_2/` -> Service 2
  - `/static/` and `/media/` -> Static and media files


## Custom Django Base Image
This project uses a custom Django base image available on Docker Hub. The image includes:
- Python 3.12
- Common dependencies for Django applications (e.g., `gunicorn`, `psycopg2`)
- Optimized for both development and production use

For more details, visit the [Docker Hub page](https://hub.docker.com/r/imansadatii/python-base).


## Environment Variables

- Each service and the database require specific environment variables. Use `.env` files to manage them.
- There are two environment: production & development.
- Make sure visit core.urls.py after switching environments.

### Example DJANGO_ENV in `.env`:
```env
DJANGO_ENV=developmnet or production
```

## Getting Started

1. Clone this repository:
   ```bash
   git clone https://github.com/imansadatii/django_base_microservice.git
   ```

2. Build and start with Makefile:
   ```bash
   make build
   ```
3. Access the services via the following routes in **production** mode:
   - **Service 1**: `http://localhost/service_1/`
   - **Service 2**: `http://localhost/service_2/`

4. Access the services via the following routes in **development** mode:
   - **Service 1**: `http://localhost:8001/`
   - **Service 2**: `http://localhost:8002/`


## Running the Application

### Using the Makefile
The `Makefile` provides convenient shortcuts for common tasks:

- **Build services:**
  ```bash
  make build
  ```
- **Start services:**
  ```bash
  make run
  ```
- **Stop services:**
  ```bash
  make down
  ```
- **log specific service:**
  ```bash
  make log SERVICE=...
  ```
- **exec specific service:**
  ```bash
  make exec SERVICE=...
  ```
- **activate specific service venv:**
  ```bash
  make venv SERVICE=...
  ```

Customize the `Makefile` as needed for additional commands.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
