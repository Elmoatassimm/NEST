# NEST System Documentation

## Introduction

System to environmental monitoring, and security in smart building environments. The system provides real-time monitoring, and implements AI-driven decision making for automated responses to environmental changes.


![Hardware](./hardware.jpg)




## System Architecture

![System Component Diagram](./System-Component.svg)

## Technology Stack

- **Backend**: Laravel PHP framework
- **Frontend**: React with TypeScript
- **UI Components**: shadcn/ui and radix-ui
- **Routing**: Inertia.js for seamless SPA experience
- **Styling**: Tailwind CSS with custom green color palette
- **Real-time Updates**: WebSockets for live data streaming
- **Security**: HMAC authentication and AES-256-CBC encryption

### **Core Modules – One Feature per Line**

1. **Access Control System:** Authenticates employees via RFID cards and logs access attempts.
2. **Access Logs:** Tracks successful and failed access events in real-time.
3. **Employee Management:** Supports photo capture/import for employee profiles.
4. **Access Notifications:** Sends instant alerts upon access events.
5. **Temperature Monitoring:** Continuously tracks and logs temperature data.
6. **Humidity Monitoring:** Monitors humidity levels and stores historical data.
7. **Threshold Alerts:** Sends alerts when environmental limits are exceeded.
9. **Cooling Trigger:** Automatically activates cooling when needed.
10. **Decision Logging:** Records the reasoning behind AI decisions.

12. **Unauthorized Access:** Detects and logs unapproved entry attempts.
13. **System Anomalies:** Identifies and alerts on abnormal system behavior.
14. **Security Alerts:** Provides real-time notifications for incidents.
15. **Incident Tracking:** Logs responses to security incidents.
16. **Manual Cooling:** Allows manual initiation of cooling systems.
17. **AI Cooling:** Enables AI-triggered climate control.
18. **Cooling Logs:** Tracks the history and outcomes of cooling actions.
19. **System Integration:** Connects with building management for coordinated control.


## Security Features

### HMAC Authentication

The system implements HMAC (Hash-based Message Authentication Code) to verify the authenticity of webhook requests

### Data Encryption

Sensitive data is protected using industry-standard encryption:

- AES-256-CBC encryption algorithm
- Base64 encoding for data transfer
- Secure key management
- Random initialization vectors (IV) for each encryption operation




### Data Flow

```mermaid
graph TD
    A[ Devices] -->|Encrypted Data| K[AI  SERVER]
    K[AI  SERVER] -->|Encrypted Data| B[ Webhook Endpoints ]
    B -->|HMAC Verification| C[Webhook Controller]
    C -->|Decryption| D[Service Layer]
    D -->|Process Data| E[Database]
    D -->|Trigger Events| F[WebSocket Server]
    F -->|Real-time Updates| G[Frontend Clients]
    H[User Interface] -->|API Requests| I[API Controllers]
    I -->|Authentication| J[Service Layer]
    J -->|CRUD Operations| E
```

## Database Schema




### Entity Relationship Diagram

```mermaid
erDiagram
    DEVICES ||--o{ ACCESS_LOGS : "has many"
    DEVICES ||--o{ TEMP_LOGS : "has many"
    DEVICES ||--o{ SECURITY_EVENTS : "has many"
    DEVICES ||--o{ COOLING_ACTIONS : "has many"
    EMPLOYEES ||--o{ ACCESS_LOGS : "has many"

    DEVICES {
        id integer PK
        name varchar
        type varchar
        status boolean
        description text
    }

    EMPLOYEES {
        id integer PK
        full_name varchar
        photo varchar
        rfid varchar
        face_data text
        fingerprint_data text
        pin_code varchar
    }

    ACCESS_LOGS {
        id integer PK
        method varchar
        success boolean
        device_id integer FK
        employee_id integer FK
    }

    TEMP_LOGS {
        id integer PK
        humidity float
        temp_c float
        device_id integer FK
    }

    SECURITY_EVENTS {
        id integer PK
        type varchar
        description text
        device_id integer FK
    }

    COOLING_ACTIONS {
        id integer PK
        action varchar
        status boolean
        device_id integer FK
    }

    AI_DECISIONS {
        id integer PK
        decision_type varchar
        reason text
    }

    USERS {
        id integer PK
        name varchar
        email varchar
        password varchar
    }
```

## Webhook Integration

The NEST system provides a robust webhook API for integrating with external systems and IoT devices. The webhook system is secured with HMAC signature verification and supports encrypted payloads.

### Webhook Endpoints

- **Main Webhook Handler**: `POST /api/webhook`
  - Processes various event types from AI system
  - Secured with HMAC signature verification
  - Supports encrypted payloads using AES-256-CBC


### HMAC Authentication

All webhook requests must include an HMAC signature for verification:

1. Calculate an HMAC-SHA256 signature using the request payload and the shared secret key
2. Add the signature to the `X-Webhook-Signature` header
3. The server verifies the signature before processing the request

```php
// Example signature calculation in PHP
$signature = hash_hmac('sha256', $payload, $secret);
```

### Encrypted Payloads

For enhanced security, webhook payloads can be encrypted:

1. Set `encrypted: true` in the request payload
2. Encrypt the data using AES-256-CBC with a shared encryption key
3. Base64 encode the encrypted data

 ## Dashboard Screenshots
 ![Dashboard Overview](./screenshots/1.png)
  ![Dashboard Overview](./screenshots/2.png)
   ![Dashboard Overview](./screenshots/3.png)
    ![Dashboard Overview](./screenshots/4.png)
     ![Dashboard Overview](./screenshots/5.png)
      ![Dashboard Overview](./screenshots/6.png) 
      ![Dashboard Overview](./screenshots/7.png)





