# Dockerizeee
# Flask Docker Application

A containerized Flask web application demonstrating Docker best practices with a complete development setup.

## 🚀 Features

- **Flask Web Application**: Simple web app with API endpoints
- **Docker Containerization**: Optimized Dockerfile with multi-stage approach
- **Health Checks**: Built-in container health monitoring
- **Security**: Non-root user execution
- **Production Ready**: Environment variables and proper logging

## 📁 Project Structure

```
flask-docker-app/
├── app.py                 # Main Flask application
├── requirements.txt       # Python dependencies
├── Dockerfile            # Container configuration
├── .dockerignore         # Docker ignore patterns
├── templates/
│   └── index.html        # Web interface
└── README.md             # This file
```

## 🛠️ Prerequisites

- Docker installed on your system
- Docker Compose (optional, for advanced usage)
- Git (for cloning the repository)

### Installing Docker

**Windows/Mac:**
- Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop)

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get update
sudo apt-get install docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker
```

## 🏃‍♂️ Quick Start

### 1. Clone or Create the Project

Create a new directory and save all the provided files:

```bash
mkdir flask-docker-app
cd flask-docker-app
```

### 2. Create the Project Structure

```bash
mkdir templates
# Copy all the provided files to their respective locations
```

### 3. Build the Docker Image

```bash
docker build -t flask-docker-app .
```

**Expected output:**
```
[+] Building 45.2s (12/12) FINISHED
 => [internal] load build definition from Dockerfile
 => [internal] load .dockerignore
 => [internal] load metadata for docker.io/library/python:3.11-slim
 => [1/7] FROM docker.io/library/python:3.11-slim
 => [2/7] WORKDIR /app
 => [3/7] COPY requirements.txt .
 => [4/7] RUN pip install --no-cache-dir -r requirements.txt
 => [5/7] COPY . .
 => [6/7] RUN useradd --create-home --shell /bin/bash app
 => [7/7] USER app
 => exporting to image
 => naming to docker.io/library/flask-docker-app
```

### 4. Run the Container

**Option A: Run in foreground (with logs)**
```bash
docker run -p 5000:5000 flask-docker-app
```

**Option B: Run in background (detached)**
```bash
docker run -d -p 5000:5000 --name my-flask-app flask-docker-app
```

**Option C: Run with environment variables**
```bash
docker run -p 5000:5000 -e PORT=5000 flask-docker-app
```

### 5. Access the Application

Open your browser and navigate to:
- **Main Application**: http://localhost:5000
- **API Status**: http://localhost:5000/api/status
- **Health Check**: http://localhost:5000/health

## 🔧 Docker Commands Reference

### Building and Running
```bash
# Build the image
docker build -t flask-docker-app .

# Run container (foreground)
docker run -p 5000:5000 flask-docker-app

# Run container (background)
docker run -d -p 5000:5000 --name my-flask-app flask-docker-app

# Run with custom port
docker run -p 8080:5000 flask-docker-app
```

### Management Commands
```bash
# List running containers
docker ps

# List all containers
docker ps -a

# Stop container
docker stop my-flask-app

# Remove container
docker rm my-flask-app

# View logs
docker logs my-flask-app

# Follow logs
docker logs -f my-flask-app

# Execute shell in container
docker exec -it my-flask-app /bin/bash
```

### Image Management
```bash
# List images
docker images

# Remove image
docker rmi flask-docker-app

# Build with no cache
docker build --no-cache -t flask-docker-app .

# Tag image
docker tag flask-docker-app:latest flask-docker-app:v1.0
```

## 🏗️ Dockerfile Explanation

The Dockerfile uses several best practices:

1. **Multi-stage approach**: Separates build dependencies from runtime
2. **Layer caching**: Copies requirements.txt first for better cache utilization
3. **Security**: Runs as non-root user
4. **Health checks**: Built-in container health monitoring
5. **Environment variables**: Configurable port and Python settings

### Key Features:
- **Base Image**: `python:3.11-slim` for smaller size
- **Working Directory**: `/app` for organization
- **User Security**: Non-root user `app`
- **Port**: Exposes port 5000
- **Health Check**: Monitors application health

## 🧪 Testing the Application

### 1. Test Endpoints

```bash
# Test main page
curl http://localhost:5000

# Test API status
curl http://localhost:5000/api/status

# Test health check
curl http://localhost:5000/health
```

### 2. Expected Responses

**API Status Response:**
```json
{
  "status": "healthy",
  "timestamp": "2024-12-19T10:30:45.123456",
  "container_id": "abc123def456",
  "message": "Flask app running in Docker container!"
}
```

**Health Check Response:**
```json
{
  "status": "ok"
}
```

## 🔍 Troubleshooting

### Common Issues

**Port already in use:**
```bash
# Use different port
docker run -p 8080:5000 flask-docker-app

# Or stop conflicting process
sudo lsof -i :5000
kill -9 <PID>
```

**Permission denied:**
```bash
# Add user to docker group (Linux)
sudo usermod -aG docker $USER
# Logout and login again
```

**Container won't start:**
```bash
# Check logs
docker logs <container-name>

# Run with interactive shell
docker run -it flask-docker-app /bin/bash
```

### Health Check Status
```bash
# Check container health
docker inspect --format='{{.State.Health.Status}}' my-flask-app
```

## 📊 Monitoring

### Container Stats
```bash
# Real-time stats
docker stats my-flask-app

# Resource usage
docker exec my-flask-app ps aux
```

### Log Monitoring
```bash
# View recent logs
docker logs --tail 50 my-flask-app

# Follow logs with timestamps
docker logs -f -t my-flask-app
```

## 🚀 Production Deployment

For production deployment, consider:

1. **Environment Variables**: Use `.env` files or orchestration tools
2. **Reverse Proxy**: Use Nginx or Apache as reverse proxy
3. **Load Balancing**: Scale with multiple container instances
4. **Secrets Management**: Use Docker Secrets or external secret management
5. **Monitoring**: Implement logging and monitoring solutions

### Docker Compose Example
```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      - PORT=5000
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
```

## 📝 Development

### Local Development
```bash
# Install dependencies locally
pip install -r requirements.txt

# Run Flask app
python app.py

# Run with debug mode
FLASK_ENV=development python app.py
```

### Making Changes
1. Modify the code
2. Rebuild the image: `docker build -t flask-docker-app .`
3. Stop and remove old container: `docker stop my-flask-app && docker rm my-flask-app`
4. Run new container: `docker run -d -p 5000:5000 --name my-flask-app flask-docker-app`

