# Signum Token Service – System Design

## Overview

The Signum Token Service is responsible for issuing, validating, refreshing, and revoking tokens for users within the Phylax IAM ecosystem. It is designed as a stateless gRPC microservice and integrates with other Phylax services such as Persona (User Service) and Patronus (Tenant Service).

## 1. gRPC API Overview

- [Signum gRPC Proto & Endpoints Overview](./grpc-api-overview.md)


## 2. Functional Requirements

- [Signum Functional Requirements](./functional-requirements.md)

## 3. Specifications

### 3.1 ID Specification

- [ID Specifications](./specs/id-spec.yaml)

### 3.2 Token Specification

- [Token Specifications](./specs/token-spec.yaml)

## 4. Database Design

* Active token table: `user_active_token`
* Revoked token table: `user_revoked_token`
* Optional summary table for quick active token count per device
* Composite key `(tenantId + userId + deviceId)` to enforce single active token per device

### ER Diagram Reference

- [Signum Database Model v1](./diagrams/er/img/signum-database-model.v1.png)
- [Signum Database Model v2](./diagrams/er/img/signum-database-model.v2.png)

## 5. System Architecture

* Stateless gRPC service deployed in Kubernetes (EKS)
* Horizontal scaling supported
* External dependencies:

  * Persona Service (user validation)
  * Patronus Service (tenant validation)
* Token storage in SQL database
* Revoked tokens retained until expiry
* Optional caching layer for active token queries

## 6. Flow Overview

1. **Token Generation**

   * AuthTokenService generates new token using validated tenantId, userId, and deviceId
   * Token stored in `user_active_token` table

2. **Token Refresh**

   * Valid refresh token request triggers new token issuance and replaces the previous token for the device

3. **Token Validation**

   * ValidationService verifies token against active token table and optionally validates signature only for stateless operation

4. **Token Revocation**

   * RevocationService moves token from active to revoked token table
   * Tokens retained until natural expiry

## 7. Notes

* gRPC services are split into three for separation of concerns
* Messages are centralized for maintainability
* Single source of truth for IDs and token specifications
* Database schema optimized for fast token lookup by `(tenantId, userId, deviceId)`

---
