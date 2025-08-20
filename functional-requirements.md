# Signum Token Service – Functional Requirements

## Overview

The Signum Token Service is responsible for issuing, validating, and revoking authentication tokens for users within the Phylax IAM system. It must manage user sessions per device, ensure stateless authentication, and support token revocation mechanisms.

## Core Functional Requirements

### 1. Token Issuance

* The service must generate a unique access token for a user upon successful authentication.
* The service must issue only one active token per combination of `(tenantId, userId, deviceId)`.
* The service must support issuing both access tokens and refresh tokens.
* Access tokens must have a configurable short expiry time (e.g., 15 minutes).
* Refresh tokens must have a longer expiry period (e.g., 7 days).

### 2. Token Validation

* The service must validate tokens based on tokenId, userId, tenantId, and deviceId.
* Token validation must confirm that the token is not expired and not present in the revoked token table.

### 3. Token Revocation

* The service must move expired or manually revoked tokens from the active tokens table to the revoked tokens table.
* The revoked tokens table must retain tokens until their natural expiry time (`expiresAt`).

### 4. Token Revocation gRPC

* The service must provide an gRPC endpoint for revoking a user token manually (e.g., logout).
* Upon revocation, the token must be removed from the active token table and added to the revoked token table.

### 5. Token Metadata

* The service must store metadata such as `issuedAt`, `expiresAt`, and optionally in the database.

## Non-Functional Requirements

### Performance & Scalability

* The service must handle high traffic and scale horizontally using distributed UUIDv7 generation.

### Security

* All tokens must be signed using a secure algorithm (e.g., JWT with HS256 or RS256).
* All communication between client and server must be encrypted over HTTPS.

### Reliability

* The system must ensure tokens cannot be reused once revoked.
* The database must handle atomic inserts and deletes for active and revoked token tables.

---