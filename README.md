# Sellora Config Server 

A **centralized configuration management server** built with **Spring Cloud Config**. It serves configuration properties to all microservices in the **Sellora** ecosystem from a single source, enabling environment-specific settings without code changes or redeployment.

-----

## 📋 Table of Contents

  - [Overview](https://www.google.com/search?q=%23overview)
  - [Architecture](https://www.google.com/search?q=%23architecture)
  - [Tech Stack](https://www.google.com/search?q=%23tech-stack)
  - [Project Structure](https://www.google.com/search?q=%23project-structure)
  - [Dependencies](https://www.google.com/search?q=%23dependencies)
  - [Configuration Sources](https://www.google.com/search?q=%23configuration-sources)
  - [Configuration Properties](https://www.google.com/search?q=%23configuration-properties)
  - [Setup & Installation](https://www.google.com/search?q=%23setup--installation)
  - [Building & Running](https://www.google.com/search?q=%23building--running)
  - [Accessing Configuration](https://www.google.com/search?q=%23accessing-configuration)
  - [Configuration Endpoints](https://www.google.com/search?q=%23configuration-endpoints)
  - [Managing Configurations](https://www.google.com/search?q=%23managing-configurations)
  - [Troubleshooting](https://www.google.com/search?q=%23troubleshooting)

-----

## 🎯 Overview

The **Config Server** is a critical infrastructure component in the **Sellora** microservices architecture. It:

  - ✅ Provides **centralized configuration management** for all microservices
  - ✅ Supports **environment-specific configurations** (dev, staging, prod)
  - ✅ Offers **dynamic configuration updates** without service redeployment
  - ✅ Enables **configuration versioning** via Git integration
  - ✅ Provides **fallback configurations** using native filesystem backend
  - ✅ Implements **single source of truth** for all **Sellora** service properties

**Application Name:** `Config-Server`  
**Default Port:** `9000`  
**Artifact ID:** `Config-Server`  
**Group ID:** `lk.ijse.eca`

-----

## 🏗️ Architecture

The Config Server sits at the core of the **Sellora** microservices configuration ecosystem:

```
┌──────────────────────────────────────────────┐
│       Spring Cloud Config Server             │
│       (Port 9000)                            │
│                                              │
│  ┌─────────────────────────────────────────┐ │
│  │ Configuration Sources:                  │ │
│  │ 1. Git Repository (Primary)             │ │
│  │ 2. Classpath/Filesystem (Fallback)      │ │
│  └─────────────────────────────────────────┘ │
└──────┬───────────────────────────────────────┘
       │
   ┌───┴─────┬──────────┬──────────┐
   │         │          │          │
   ▼         ▼          ▼          ▼
┌─────┐ ┌──────┐ ┌──────┐ ┌──────┐
│API  │ │User  │ │Item  │ │Order │
│Gate │ │Svc   │ │Svc   │ │Svc   │
│way  │ └──────┘ └──────┘ └──────┘
└─────┘
 (All Sellora services fetch configs on startup)

┌──────────────────────────────────┐
│     Sellora Config Repository    │
│ (GitHub / Local Git Repo)        │
└──────────────────────────────────┘
```

-----

## 🛠️ Tech Stack

| Component | Version | Purpose |
|-----------|---------|---------|
| **Java** | JDK 21 | Language runtime |
| **Spring Boot** | 4.0.3 | Application framework |
| **Spring Cloud** | 2025.1.0 | Microservices framework |
| **Config Server** | Latest | Configuration management engine |
| **Git Integration** | Latest | Version control for configurations |
| **Actuator** | Latest | Health checks & monitoring |

-----

## ⚙️ Configuration Sources

The **Sellora** Config Server supports two configuration sources in order of priority:

### 1\. Git Repository (Primary)

**Configuration:**

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/Sellora-Org/Sellora-Configurations.git
          search-paths: platform,services
```

### 2\. Classpath/Filesystem Backend (Fallback)

**Benefits:**

  - ✅ Works without external Git repository (Offline mode)
  - ✅ Useful for local development of **Sellora** services
  - ✅ Fast startup time

-----

## 🚀 Setup & Installation

### Prerequisites

  - **Java 21**
  - **Maven 3.6+**
  - **Git** (for version control)

### Installation Steps

1.  **Clone the repository**

    ```bash
    git clone <repository-url>
    cd Sellora-Project/platform/config-server
    ```

2.  **Build the project**

    ```bash
    ./mvnw clean install
    ```

-----

## ▶️ Building & Running

### Startup Verification

**⚠️ Critical:** Config Server should start **FIRST** before any other service in the **Sellora** platform.

**Run:**

```bash
./mvnw spring-boot:run
```

**Check Health:**

```bash
curl http://localhost:9000/actuator/health
```

-----

## 📥 Accessing Configuration

Microservices fetch their configuration via the following REST format:

`http://localhost:9000/{application}/{profile}/{label}`

**Example for Sellora User Service (Dev):**

```bash
curl http://localhost:9000/user-service/dev
```

-----

## 📊 Typical Configuration Flow

1.  **Sellora Service Startup**
      - Service reads `spring.config.import: "configserver:"`
      - Sends request to **Config Server** (port 9000).
2.  **Config Resolution**
      - Config Server fetches properties from Git or local classpath.
3.  **Property Injection**
      - Service receives properties and completes initialization.

-----

## 🤝 Support

For issues or questions regarding **Sellora** centralized configuration:

1.  Ensure the Config Server is running before starting the **Service Registry** or **API Gateway**.
2.  Verify the Git URI is accessible.
3.  Check the `search-paths` if a new service configuration is not being found.

-----

Would you like me to create the initial `user-service.yaml` and `item-service.yaml` files for your **Sellora** configuration repository?
