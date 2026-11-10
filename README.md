
# 📝 Blog Platform

A Containerized Multi-Tier Blogging Application using Node.js, Express.js, MongoDB, Nginx, Docker and Docker Compose.

## 📌 Project Overview

Blog Platform is a multi-container web application that allows users to create and view blog posts.

The application follows a multi-tier architecture where the frontend, backend, reverse proxy, and database run as separate Docker containers.

Docker Compose is used to manage the complete application stack and simplify deployment.

## 🎯 Problem Statement

Deploying a web application using a single server or manually managing multiple application components can create several challenges:

- Difficult to manage frontend, backend, and database independently.
- Application dependencies may differ between environments.
- Directly exposing backend services can create security concerns.
- Manual deployment and configuration can become complex.
- Database data may be lost when containers are removed without persistent storage.

## 💡 Solution

To address these challenges, I implemented a containerized architecture using Docker and Docker Compose.

Each application component runs in an isolated container and communicates through a dedicated Docker network.

Nginx acts as a reverse proxy and routes incoming requests to the appropriate application service.

MongoDB uses a persistent Docker volume to maintain database records.

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| HTML5 | Frontend structure |
| CSS3 | Frontend styling |
| JavaScript | Client-side functionality |
| Node.js | Backend runtime |
| Express.js | REST API development |
| MongoDB | NoSQL database |
| Mongoose | MongoDB object modeling |
| Nginx | Reverse proxy |
| Docker | Application containerization |
| Docker Compose | Multi-container orchestration |
| Git & GitHub | Version control |

## 🏗️ System Architecture

The application follows a multi-tier architecture consisting of four main services:

1. Nginx – Reverse Proxy
2. Frontend – HTML, CSS and JavaScript
3. Backend – Node.js and Express.js
4. Database – MongoDB

### Architecture Diagram

```text
                  +------------------+
                  |   User / Browser |
                  +------------------+
                           |
                           |
                       Port 8080
                           |
                           v
                 +--------------------+
                 |        NGINX       |
                 |    Reverse Proxy   |
                 +--------------------+
                       /        \
                      /          \
                     v            v
          +----------------+  +------------------+
          |    Frontend    |  |     Backend      |
          |   HTML/CSS/JS  |  |  Node.js/Express |
          |    Port 80     |  |    Port 5000     |
          +----------------+  +------------------+
                                      |
                                      |
                                      v
                              +----------------+
                              |    MongoDB     |
                              |   Port 27017   |
                              +----------------+
                                      |
                                      v
                              +----------------+
                              | Docker Volume  |
                              |   mongo-data   |
                              +----------------+
```

### 1. Nginx – Reverse Proxy

Nginx acts as the entry point for incoming application traffic.

Responsibilities:

- Listens on port 8080 of the host machine.
- Routes frontend requests to the frontend container.
- Routes `/api/` requests to the backend container.
- Provides a single entry point for application services.

### 2. Frontend – HTML, CSS and JavaScript

The frontend provides the user interface.

Responsibilities:

- Displays the blogging application.
- Allows users to create blog posts.
- Fetches existing blog posts from the backend.
- Communicates with backend REST APIs.

### 3. Backend – Node.js and Express.js

The backend handles application logic and API requests.

Responsibilities:

- Provides REST API endpoints.
- Processes incoming blog creation requests.
- Retrieves blog posts from MongoDB.
- Validates incoming data.
- Handles API errors.

Mongoose is used to interact with MongoDB.

### 4. MongoDB – Database

MongoDB stores the application's blog post data.

Responsibilities:

- Stores blog titles and content.
- Maintains creation and update timestamps.
- Provides persistent data storage through Docker volumes.

### 5. Docker Compose

Docker Compose manages all application services.

Responsibilities:

- Builds frontend and backend images.
- Creates and manages containers.
- Configures container networking.
- Manages service dependencies.
- Configures MongoDB health checks.
- Maintains persistent database storage.

## 🔄 Application Request Flow

The application follows this request-response flow:

1. User accesses the application through a web browser.
2. The request reaches the Nginx reverse proxy.
3. Nginx forwards frontend requests to the frontend container.
4. The frontend sends API requests to the backend.
5. The backend processes the incoming request.
6. The backend communicates with MongoDB.
7. MongoDB stores or retrieves blog data.
8. The backend returns the response.
9. The frontend displays the blog posts to the user.

## 📂 Project Structure

```text
Blog-Platform/
│
├── backend/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
│
├── frontend/
│   ├── index.html
│   └── Dockerfile
│
├── nginx/
│   └── nginx.conf
│
├── docker-compose.yml
│
├── .gitignore
│
└── README.md
```

## 🐳 Docker Implementation

Each application component is containerized separately.

### Docker Services

| Service | Container | Internal Port |
|---|---|---|
| Nginx | blog-nginx | 80 |
| Frontend | blog-frontend | 80 |
| Backend | blog-backend | 5000 |
| MongoDB | blog-mongo | 27017 |

The application uses Docker Compose to build and manage all services.

### Docker Networking

All containers communicate through the Docker Compose default network.

Nginx communicates with frontend and backend using their service names.

Backend communicates with MongoDB using the MongoDB service name.

### Persistent Storage

MongoDB uses a named Docker volume:

```yaml
volumes:
  mongo-data:
```

This ensures database records remain available when containers are recreated.

## ⚙️ Prerequisites

Before running the project, install:

- Git
- Docker
- Docker Compose
- Visual Studio Code (optional)

## 🚀 Installation and Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/lalita-15/Blog-Platform.git

cd Blog-Platform
```

### Step 2: Build and Start Containers

```bash
docker compose up --build -d
```

This command builds the application images and starts all services in detached mode.

### Step 3: Verify Running Containers

```bash
docker compose ps
```

Check that all four services are running.

### Step 4: Check Application Logs

```bash
docker compose logs -f
```

To check an individual service:

```bash
docker compose logs backend

docker compose logs nginx

docker compose logs mongo
```

### Step 5: Access the Application

Open your browser:

```text
http://localhost:8080
```

## 🔌 API Endpoints

The backend exposes the following REST API endpoints:

| Method | Endpoint | Description |
|---|---|---|
| GET | /api/blogs | Retrieve all blog posts |
| POST | /api/blogs | Create a new blog post |
| GET | /health | Check backend health |

### Example: Create a Blog Post

```bash
curl -X POST http://localhost:8080/api/blogs \
-H "Content-Type: application/json" \
-d '{"title":"My First Blog","content":"Learning Docker and DevOps"}'
```

### Example: Retrieve Blog Posts

```bash
curl http://localhost:8080/api/blogs
```

## 🩺 Health Checks and Service Dependencies

MongoDB includes a Docker health check to verify database availability.

The backend service depends on MongoDB being healthy before starting.

This helps prevent the backend from starting before the database is ready.

Backend health endpoint:

```text
http://localhost:8080/health
```

## 🔍 Troubleshooting

### 1. Container Not Starting

Check container status:

```bash
docker compose ps -a
```

Check logs:

```bash
docker compose logs
```

### 2. Backend Cannot Connect to MongoDB

Check MongoDB logs:

```bash
docker compose logs mongo
```

Verify the MongoDB connection string:

```text
mongodb://mongo:27017/blogdb
```

### 3. Nginx Bad Gateway

Check backend and frontend containers:

```bash
docker compose ps
```

Check Nginx configuration and logs:

```bash
docker compose logs nginx
```

### 4. Rebuild Containers

```bash
docker compose down

docker compose up --build -d
```

## 🛑 Stop the Application

To stop and remove application containers:

```bash
docker compose down
```

To remove containers and the MongoDB data volume:

```bash
docker compose down -v
```

**Warning:** The second command deletes the persistent MongoDB volume and its stored data.

## 📚 Key Learnings

Through this project, I gained practical experience in:

- Designing a multi-tier application architecture.
- Containerizing frontend and backend applications.
- Creating and managing Dockerfiles.
- Deploying multiple services using Docker Compose.
- Configuring Nginx as a reverse proxy.
- Managing container-to-container communication.
- Integrating Node.js with MongoDB.
- Implementing persistent storage using Docker volumes.
- Configuring health checks and service dependencies.
- Troubleshooting container and networking issues.

## 🚀 Future Enhancements

The following improvements can be considered for future development:

- Jenkins CI/CD pipeline.
- Automated Docker image build and push.
- Kubernetes deployment.
- Kubernetes Services and Ingress.
- Prometheus and Grafana monitoring.
- Centralized application logging.
- Authentication and user management.
- Blog update and delete functionality.
- HTTPS configuration and production security hardening.

## 🔗 Repository

GitHub: https://github.com/lalita-15/Blog-Platform

## 👩‍💻 Author

**Lalita Rajpoot**

Cloud & DevOps Engineer

GitHub: https://github.com/lalita-15

LinkedIn: https://www.linkedin.com/in/lalita-rajpoot-05487521a

---



**Built with ❤️ using Docker, Node.js, MongoDB and Nginx.**
