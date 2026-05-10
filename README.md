# Parking Docker Deployment

This folder contains Docker Compose files for local and EC2 deployment.

## Database Migrations

Database schema changes are managed by Flyway inside the Spring Boot services, not by Docker init scripts.

Service-owned migrations live here:

| Service | Migration folder | Flyway history table |
| --- | --- | --- |
| parking-auth-service | `src/main/resources/db/migration` | `flyway_schema_history_auth` |
| parking-location-service | `src/main/resources/db/migration` | `flyway_schema_history_location` |
| parking-booking-service | `src/main/resources/db/migration` | `flyway_schema_history_booking` |
| parking-payment-service | `src/main/resources/db/migration` | `flyway_schema_history_payment` |

Docker only starts PostgreSQL. When each service starts, Flyway applies that service's migrations before JPA validates the schema.

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
| Parking Location Swagger | `http://<ec2-public-ip>:8083/swagger-ui.html` |
| Parking Booking Swagger | `http://<ec2-public-ip>:8084/swagger-ui.html` |
| Parking Payment Swagger | `http://<ec2-public-ip>:8085/swagger-ui.html` |

PostgreSQL is not exposed publicly in the EC2 compose file. It is only reachable by containers on the `parking-network` Docker network.