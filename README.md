# Parking Docker Deployment

This folder contains Docker Compose files for local and EC2 deployment.

## EC2 Compose

Copy `.env.example` to `.env` and replace the passwords/secrets before starting services.

```bash
cp .env.example .env
nano .env
```

Start the full stack:

```bash
docker compose -f docker-compose.ec2.yml up -d --build
```

Services:

| Service | URL |
| --- | --- |
| Parking API Swagger | `http://<ec2-public-ip>:8081/swagger-ui.html` |
| Parking Auth Swagger | `http://<ec2-public-ip>:8082/swagger-ui.html` |

PostgreSQL is not exposed publicly in the EC2 compose file. It is only reachable by containers on the `parking-network` Docker network.