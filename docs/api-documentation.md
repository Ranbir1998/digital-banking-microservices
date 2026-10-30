# REST API Documentation

**Project:** Digital Banking System
**Documentation:** Ranbir Singh

## Overview

The Digital Banking System exposes REST APIs through its Spring Boot microservices.

The API Gateway runs on port `8080`.

Individual services are also accessible through their configured local development ports.

## Service Endpoints

| Service                 | Local Base URL          |
| ----------------------- | ----------------------- |
| API Gateway             | `http://localhost:8080` |
| Account Service         | `http://localhost:8081` |
| Transaction Service     | `http://localhost:8082` |
| Payment Service         | `http://localhost:8083` |
| Fraud Detection Service | `http://localhost:8084` |
| Notification Service    | `http://localhost:8085` |

## Account Service

The following endpoints have been identified from the existing project.

| Method | Endpoint                                   | Purpose                      |
| ------ | ------------------------------------------ | ---------------------------- |
| POST   | `/api/v1/accounts`                         | Create an account            |
| GET    | `/api/v1/accounts/{accountNumber}`         | Retrieve account information |
| GET    | `/api/v1/accounts/{accountNumber}/balance` | Retrieve account balance     |
| PUT    | `/api/v1/accounts/{accountNumber}/block`   | Block an account             |
| PUT    | `/api/v1/accounts/{accountNumber}/deduct`  | Deduct an amount             |
| PUT    | `/api/v1/accounts/{accountNumber}/credit`  | Credit an amount             |

### Create Account

**Request**

```http
POST /api/v1/accounts
Content-Type: application/json
```

**Example JSON**

```json
{
  "accountHolderName": "Test Customer",
  "email": "test.customer@example.com",
  "phone": "9876543210",
  "accountType": "SAVINGS",
  "initialDeposit": 10000
}
```

The `accountType` value must match an enum constant defined by the application. The example uses `SAVINGS`, which should be confirmed against the enum.

### Create Account Validation

The request DTO defines the following validation rules:

| Field               | Validation                   |
| ------------------- | ---------------------------- |
| `accountHolderName` | Required, nonblank           |
| `email`             | Required, valid email format |
| `phone`             | Required, nonblank           |
| `accountType`       | Required                     |
| `initialDeposit`    | Required, positive number    |

### Retrieve Account

```http
GET /api/v1/accounts/{accountNumber}
```

### Retrieve Balance

```http
GET /api/v1/accounts/{accountNumber}/balance
```

### Block Account

```http
PUT /api/v1/accounts/{accountNumber}/block
```

### Deduct Amount

```http
PUT /api/v1/accounts/{accountNumber}/deduct?amount=100
```

### Credit Amount

```http
PUT /api/v1/accounts/{accountNumber}/credit?amount=100
```

## Transaction Service

Responsible for transaction-processing operations and transaction history.

Exact REST routes and request schemas require confirmation from the Transaction Service controllers.

## Payment Service

Responsible for payment integration and webhook processing.

Exact REST routes, webhook validation behavior, and response schemas require confirmation from the Payment Service controllers.

## Fraud Detection Service

Processes fraud-related events and evaluates transaction activity.

Any externally exposed REST endpoints require confirmation from the service controllers.

## Notification Service

Processes notification-related events.

Any externally exposed REST endpoints require confirmation from the service controllers.

## API Verification Status

The Account Service health endpoint has been verified.

The account creation, balance, transfer, payment, and fraud-detection operations have not yet been functionally tested.

API documentation will be expanded after controller routes and response models are reviewed.
