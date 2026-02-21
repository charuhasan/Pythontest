# Dockerized Python App Sample

A sample Flask application containerized with Docker. This demonstrates best practices for dockerizing Python applications.

## Project Structure

```
.
├── app.py              # Flask application
├── requirements.txt    # Python dependencies
├── Dockerfile          # Docker configuration
├── .dockerignore       # Docker ignore rules
└── README.md          # This file
```

## Features

- **Flask Web Server**: Simple REST API with multiple endpoints
- **Health Check**: Built-in health check endpoint for container monitoring
- **Production-Ready**: Slim Python base image, proper signal handling
- **Optimized**: Multi-layer caching for faster rebuilds

## API Endpoints

- `GET /` - Welcome message
- `GET /api/info` - Application information
- `GET /health` - Health check (used by Docker)

## Building the Docker Image

```bash
docker build -t python-app:1.0 .
```

## Running the Container

### Basic Usage
```bash
docker run -p 5000:5000 python-app:1.0
```

### With Environment Variable
```bash
docker run -p 5000:5000 -e ENVIRONMENT=staging python-app:1.0
```

### Running in Background
```bash
docker run -d -p 5000:5000 --name my-python-app python-app:1.0
```

## Testing the App

```bash
# Welcome endpoint
curl http://localhost:5000/

# Info endpoint
curl http://localhost:5000/api/info

# Health check
curl http://localhost:5000/health
```

## Stopping the Container

```bash
docker stop my-python-app
docker rm my-python-app
```

## Local Development (Without Docker)

```bash
pip install -r requirements.txt
python app.py
```

Then visit `http://localhost:5000` in your browser.

## Docker Compose (Optional)

Create a `docker-compose.yml` file for easier management:

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "5000:5000"
    environment:
      - ENVIRONMENT=production
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 5s
```

Run with: `docker-compose up`

## Requirements

- Docker Desktop (Windows/Mac) or Docker Engine (Linux)
- Python 3.11+ (for local development)

## Notes

- The app runs on `0.0.0.0:5000` to be accessible from outside the container
- Production mode is enabled in the Docker container
- The slim Python image reduces the final image size
