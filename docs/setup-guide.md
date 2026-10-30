# Local Development Setup

**Project:** Digital Banking System
**Documentation:** Ranbir Singh

## Prerequisites

Install:

- Docker Desktop
- Docker Compose
- Git

Java 17 and Maven are optional when building and running the services entirely through Docker.

## 1. Obtain the Project

Clone the repository from a location where you have permission to access the source code.

Open a terminal in the project root directory.

## 2. Configure Environment Variables

Create a `.env` file in the project root.

Use `.env.example` as the configuration template.

The Docker Compose configuration references:

| Variable                  | Purpose                              |
| ------------------------- | ------------------------------------ |
| `MYSQL_ROOT_PASSWORD`     | Local MySQL root password            |
| `RAZORPAY_KEY_ID`         | Razorpay integration key identifier  |
| `RAZORPAY_KEY_SECRET`     | Razorpay integration secret          |
| `RAZORPAY_WEBHOOK_SECRET` | Razorpay webhook verification secret |

Do not commit real passwords or payment credentials.

## 3. Validate Docker Compose

```bash
docker compose config --quiet
```

The command should complete without errors.

## 4. Build Application Images

```bash
docker compose build
```

The project defines six locally built application images:

- `banking/account-service:local`
- `banking/transaction-service:local`
- `banking/payment-service:local`
- `banking/fraud-detection-service:local`
- `banking/notification-service:local`
- `banking/api-gateway:local`

## 5. Start the Application

```bash
docker compose up -d
```

## 6. Check Container Status

```bash
docker compose ps
```

The development environment includes ten containers.

| Component               | Host Port |
| ----------------------- | --------- |
| API Gateway             | 8080      |
| Account Service         | 8081      |
| Transaction Service     | 8082      |
| Payment Service         | 8083      |
| Fraud Detection Service | 8084      |
| Notification Service    | 8085      |
| Kafka                   | 9092      |
| MySQL                   | 3306      |
| Redis                   | 6380      |

ZooKeeper is available on the internal Docker network.

## 7. Check Account Service Health

```powershell
Invoke-RestMethod http://localhost:8081/actuator/health
```

In the verified local environment, the endpoint returned:

```json
{
  "status": "UP"
}
```

## 8. View Application Logs

```bash
docker compose logs --tail=50 account-service
```

To follow logs continuously:

```bash
docker compose logs -f
```

## 9. Stop the Application

```bash
docker compose down
```

This stops and removes the containers while retaining named volumes.

To remove volumes as well, use:

```bash
docker compose down -v
```

**Warning:** Removing volumes deletes locally persisted MySQL data.

## Troubleshooting

### Port Already in Use

If Docker reports a port conflict, check whether another process or container is using the required port.

### Database Connection Errors

Verify that MySQL is running and that the configured credentials match.

### Kafka Connection Errors

Applications running inside Docker use the Kafka hostname `kafka` and port `29092`.

Applications running directly on the host use `localhost:9092`.

### Redis Connection Errors

Applications running inside Docker connect to `redis:6379`.

The configured host port is `6380`.

### Service Restarting

Inspect the service logs:

```bash
docker compose logs --tail=100 <service-name>
```

## Verified Environment

The application images built successfully using Docker Compose.

All ten containers were observed running, and the Account Service health endpoint returned `UP`.

This does not establish that every application workflow has passed functional testing.
