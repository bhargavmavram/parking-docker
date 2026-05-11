# Parking Platform API Catalog

All protected APIs use a stateless JWT from `parking-auth-service`. Send it as:

```http
Authorization: Bearer <accessToken>
```

Refresh tokens are not implemented at this stage.

## Services and Swagger

| Service | Port | Swagger |
| --- | ---: | --- |
| parking-api-service | 8081 | http://localhost:8081/swagger-ui/index.html |
| parking-auth-service | 8082 | http://localhost:8082/swagger-ui/index.html |
| parking-location-service | 8083 | http://localhost:8083/swagger-ui/index.html |
| parking-booking-service | 8084 | http://localhost:8084/swagger-ui/index.html |
| parking-payment-service | 8085 | http://localhost:8085/swagger-ui/index.html |

## Roles

Current roles are `ADMIN`, `USER`, `MANAGER`, `EMPLOYEE`, `PARKING_OWNER`, `PAYMENT_TESTER`, and `SUPPORT`.

## Auth Service

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| POST | `/auth/register` | Public | Register user and assign roles. Defaults to `USER` if roles are omitted. |
| POST | `/auth/login` | Public | Login and receive JWT access token. |
| GET | `/status` | Public | Service health/status. |

## API Service

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| GET | `/status` | Public | Service health/status. Alias for `/api/status`. |
| GET | `/api/status` | Public | Service health/status. |
| GET | `/api/secure/sample` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, PAYMENT_TESTER, SUPPORT | Secured sample endpoint. |
| GET | `/api/secure/hello` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, PAYMENT_TESTER, SUPPORT | Alias for secured sample endpoint used by the Postman collection. |
## Location Service

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| GET | `/status` | Public | Service health/status. |
| GET | `/zones` | ADMIN, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | List parking zones. |
| POST | `/zones` | ADMIN, MANAGER, PARKING_OWNER | Create a parking zone. |
| GET | `/zones/{zoneId}/availability` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | Get total and available spaces for a zone. |
| GET | `/parking-locations?zoneId={zoneId}` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | List parking locations, optionally filtered by zone. |
| POST | `/parking-locations` | ADMIN, MANAGER, PARKING_OWNER | Create a parking location. Supports unmapped, mapped, mall-camera, street-zone and other types. |
| GET | `/parking-locations/{locationId}/spaces` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | List spaces for a location. |
| POST | `/parking-locations/{locationId}/spaces` | ADMIN, MANAGER, PARKING_OWNER, EMPLOYEE | Create a parking space. |
| PATCH | `/spaces/{spaceId}/status` | ADMIN, MANAGER, PARKING_OWNER, EMPLOYEE | Update space status. |
| GET | `/parking-locations/{locationId}/availability` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | Get total and available spaces for a location. |
| POST | `/parking-locations/{locationId}/cameras` | ADMIN, MANAGER, PARKING_OWNER | Create/register a mock camera device. |
| POST | `/cameras/{cameraId}/plate-events` | ADMIN, MANAGER, EMPLOYEE, PARKING_OWNER | Upload an image as multipart field `image`; returns a random mock vehicle number plate. |

## Booking Service

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| GET | `/status` | Public | Service health/status. |
| POST | `/bookings` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER | Create a parking booking for the authenticated user. |
| GET | `/bookings` | ADMIN, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | List bookings, optionally filtered by `locationId`. |
| GET | `/bookings/me` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER | List bookings for the authenticated user. |
| GET | `/bookings/{bookingId}` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | Get booking by id. |
| POST | `/bookings/{bookingId}/confirm` | ADMIN, MANAGER, EMPLOYEE, PARKING_OWNER | Confirm a booking. |
| POST | `/bookings/{bookingId}/cancel` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | Cancel a booking. |
| POST | `/sessions/start` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER | Start an ad-hoc parking session. |
| POST | `/sessions/start-with-booking/{bookingId}` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER | Start a session from a confirmed booking. |
| POST | `/sessions/start-by-plate` | ADMIN, MANAGER, EMPLOYEE, PARKING_OWNER | Start a session using a vehicle registration number from a camera flow. |
| POST | `/sessions/{sessionId}/end` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | End an active session. |
| GET | `/sessions/active` | ADMIN, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | List active sessions. |
| GET | `/sessions/by-vehicle/{registrationNumber}` | ADMIN, MANAGER, EMPLOYEE, PARKING_OWNER, SUPPORT | Find sessions by vehicle registration number. |

## Payment Service

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| GET | `/status` | Public | Service health/status. |
| POST | `/test-cards` | ADMIN, PAYMENT_TESTER | Create a mock payment card. |
| GET | `/test-cards` | ADMIN, PAYMENT_TESTER, SUPPORT | List mock cards. |
| PATCH | `/test-cards/{cardId}/balance` | ADMIN, PAYMENT_TESTER | Update card balance. |
| POST | `/test-cards/{cardId}/expire` | ADMIN, PAYMENT_TESTER | Mark card as expired. |
| POST | `/test-cards/{cardId}/block` | ADMIN, PAYMENT_TESTER | Mark card as blocked. |
| POST | `/payment-rules` | ADMIN, PAYMENT_TESTER | Create mock payment behavior rule. |
| GET | `/payment-rules` | ADMIN, PAYMENT_TESTER, SUPPORT | List payment rules. |
| POST | `/payments/intent` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER | Create payment intent for a booking/session. |
| POST | `/payments/{paymentId}/confirm` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER | Confirm a mock payment. Debits balance on success. |
| POST | `/payments/{paymentId}/refund` | ADMIN, MANAGER, PAYMENT_TESTER, SUPPORT | Refund a succeeded mock payment. |
| GET | `/payments` | ADMIN, MANAGER, PAYMENT_TESTER, SUPPORT | List all payments. |
| GET | `/payments/me` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER | List current user's payments. |
| GET | `/payments/{paymentId}` | ADMIN, USER, MANAGER, EMPLOYEE, PARKING_OWNER, PAYMENT_TESTER, SUPPORT | Get payment by id. |