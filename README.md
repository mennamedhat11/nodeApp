# Node App: Express + Docker + Jenkins CI/CD

A lightweight **Node.js (Express)** REST API, containerized with **Docker** and deployed through a **Jenkins** multi-environment pipeline (dev / staging / production) with an automated smoke test.

![Node.js](https://img.shields.io/badge/Node.js-339933?logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?logo=express&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?logo=jenkins&logoColor=white)

## API Endpoints

| Method | Endpoint  | Description                  |
|--------|-----------|------------------------------|
| GET    | `/`       | Welcome message             |
| GET    | `/health` | Health check, returns status |

Example:

```bash
curl http://localhost:3000/health
```

## Project Structure

```
.
├── app.js           # Express application
├── package.json     # Dependencies and scripts
├── Dockerfile       # Image build instructions
├── .dockerignore    # Files excluded from the image
├── Jenkinsfile      # CI/CD pipeline definition
└── README.md
```

## Run Locally

```bash
npm install
npm start
```

Open http://localhost:3000

## Run with Docker

```bash
docker build -t node-app .
docker run -d -p 3000:3000 node-app
```

## CI/CD Pipeline

The `Jenkinsfile` runs these stages on every build:

| # | Stage | Description |
|---|-------|-------------|
| 1 | **Clone** | Checks out the source code |
| 2 | **Set Environment** | Picks the environment, container name, port and image tag based on the branch |
| 3 | **Build Image** | Builds the Docker image with a versioned tag and an environment `latest` tag |
| 4 | **Push Image** | Logs in to Docker Hub with Jenkins credentials and pushes both tags |
| 5 | **Deploy** | Replaces the running container and starts the new one with `--restart unless-stopped` |
| 6 | **Smoke Test** | Calls `/health` inside the container to verify the deployment |

After the run, unused Docker images are pruned automatically.

### Branch-based environments

| Branch | Environment | Host Port | Image Tag |
|--------|-------------|-----------|-----------|
| `prod` | production  | 3002 | `prod-<build>` / `prod-latest` |
| `stg`  | staging     | 3001 | `stg-<build>` / `stg-latest` |
| any other (e.g. `main`) | dev | 3000 | `dev-<build>` / `dev-latest` |

The environment name is also passed to the container as the `APP_ENV` variable.

## Jenkins Setup

1. Install Docker on the Jenkins agent and allow the `jenkins` user to run it.
2. Add a **Username/Password** credential with ID `Docker` (Docker Hub).
3. Create a **Multibranch Pipeline** job pointing to this repository.

## Author

**Menna Medhat**
[GitHub](https://github.com/mennamedhat11)
