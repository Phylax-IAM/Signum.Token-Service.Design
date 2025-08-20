# Signum Token Service – gRPC API Overview

## Overview

This document outlines the gRPC service definitions, proto message structures, and endpoint specifications for the Signum Token Service. The service exposes four main RPC methods, now organized into three separate services:

* AuthTokenService: GenerateNewToken, RefreshToken
* ValidationService: ValidateToken
* RevocationService: RevokeToken (Logout)

---

## Proto Files Structure

### 1. token\_messages.proto

```proto
syntax = "proto3";
package signum;

message GenerateTokenRequest {
  string tenant_id = 1; // UUIDv7
  string user_id   = 2; // UUIDv7
  string device_id = 3; // UUIDv7 (server-issued)
  string client_id = 4; // optional, registered app/client
}

message GenerateTokenResponse {
  string access_token  = 1;
  string refresh_token = 2;
  string expires_at    = 3; // ISO timestamp
}

message RefreshTokenRequest {
  string refresh_token = 1;
  string device_id     = 2;
}

message RefreshTokenResponse {
  string access_token  = 1;
  string refresh_token = 2;
  string expires_at    = 3;
}

message ValidateTokenRequest {
  string access_token = 1;
}

message ValidateTokenResponse {
  bool valid      = 1;
  string tenant_id = 2;
  string user_id   = 3;
  string device_id = 4;
  string message   = 5; // success or reason of failure
}

message RevokeTokenRequest {
  string token_id = 1; // could be access or refresh token
  string device_id = 2; // optional
}

message RevokeTokenResponse {
  bool success = 1;
  string message = 2;
}
```

### 2. auth\_token\_service.proto

```proto
syntax = "proto3";
package signum;

import "token_messages.proto";

service AuthTokenService {
  rpc GenerateNewToken(GenerateTokenRequest) returns (GenerateTokenResponse);
  rpc RefreshToken(RefreshTokenRequest) returns (RefreshTokenResponse);
}
```

### 3. validation\_service.proto

```proto
syntax = "proto3";
package signum;

import "token_messages.proto";

service ValidationService {
  rpc ValidateToken(ValidateTokenRequest) returns (ValidateTokenResponse);
}
```

### 4. revocation\_service.proto

```proto
syntax = "proto3";
package signum;

import "token_messages.proto";

service RevocationService {
  rpc RevokeToken(RevokeTokenRequest) returns (RevokeTokenResponse);
}
```

---

## Endpoint Specification Table

| RPC Method           | Service Name      | Request Message      | Response Message      | Description                                              |
| -------------------- | ----------------- | -------------------- | --------------------- | -------------------------------------------------------- |
| GenerateNewToken     | AuthTokenService  | GenerateTokenRequest | GenerateTokenResponse | Issues new access + refresh token for authenticated user |
| RefreshToken         | AuthTokenService  | RefreshTokenRequest  | RefreshTokenResponse  | Refresh workflow to issue new tokens using refresh token |
| ValidateToken        | ValidationService | ValidateTokenRequest | ValidateTokenResponse | Validates access token and extracts tenant + user info   |
| RevokeToken (Logout) | RevocationService | RevokeTokenRequest   | RevokeTokenResponse   | Revokes a token and moves it to revoked table            |

---

## Notes & Future Enhancements

* DeviceId is required to validate refresh requests (ensures binding)
* Access token validation may become stateless (verify signature only)
* For future, add metadata in responses (scope/roles)
* Messages are now centralized in `token_messages.proto` for reuse across all services

---
