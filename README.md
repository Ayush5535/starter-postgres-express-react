# Dockerized PERN Application on AWS EC2

## Project Overview

This project is a 3-tier PERN stack application consisting of:

- Frontend: React + Nginx
- Backend: Node.js + Express
- Database: PostgreSQL 16 Alpine
- Containerization: Docker and Docker Compose
- Deployment: AWS EC2 Ubuntu

The original open-source application was containerized and deployed on an AWS EC2 instance using Docker Compose.

## System Architecture

The application follows a 3-tier architecture:

Browser → React/Nginx Frontend → Node.js/Express Backend → PostgreSQL Database

All services communicate through a private Docker network named `app-network`.

## Docker Services

| Service | Technology | Container Port | Host Port |
|---|---|---:|---:|
| Frontend | React + Nginx | 80 | 3000 |
| Backend | Node.js + Express | 5000 | 5000 |
| Database | PostgreSQL 16 Alpine | 5432 | Not exposed |

PostgreSQL is not exposed to the public internet. It is accessible only by the backend through the Docker network.

## Persistent Data

PostgreSQL uses a named Docker volume:

`postgres_data`

The volume ensures database data persists even when the PostgreSQL container is recreated.

## Docker Networking

Docker Compose creates a custom network:

`app-network`

The frontend communicates with the backend, and the backend communicates with PostgreSQL using Docker service names instead of hardcoded IP addresses.

## Deployment on AWS EC2

The application was deployed on an Ubuntu AWS EC2 instance.

Build and start all services:

```bash
docker compose up -d --build

Check running containers:

docker compose ps

Check backend logs:

docker compose logs --tail=30 backend

Test the backend:

curl -I http://localhost:5000

The backend returned HTTP 200 OK during deployment testing.

Security Group

The EC2 security group allows:

SSH: TCP 22
Frontend: TCP 3000
Backend API: TCP 5000

PostgreSQL port 5432 is not exposed through the EC2 security group.

Application Access

Frontend:

http://<EC2-PUBLIC-IP>:3000

Backend API:

http://<EC2-PUBLIC-IP>:5000
Technologies Used
React
Nginx
Node.js
Express
PostgreSQL 16
Docker
Docker Compose
AWS EC2
Ubuntu
Git & GitHub
Dockerization

Separate Dockerfiles are used for the frontend and backend.

The frontend uses a production build served through Nginx.

The backend runs the production Node.js/Express server.

PostgreSQL runs using the lightweight postgres:16-alpine image.

Persistence

A named Docker volume is used for PostgreSQL:

postgres_data

This keeps database data available even if the database container is removed and recreated.

Repository Changes

The main DevOps changes include:

Added Dockerfile.frontend
Added Dockerfile.backend
Added docker-compose.yml
Added Nginx configuration
Added Docker networking
Added PostgreSQL persistent volume
Configured AWS EC2 deployment
Documented security group and exposed ports
Added deployment and verification commands
Conclusion

The application was successfully containerized using Docker Compose and deployed on AWS EC2.

The final architecture separates the frontend, backend, and database into independent containers while using Docker networking and persistent storage for reliable deployment.
