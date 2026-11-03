# Host Monitoring API (Spring Boot)

A REST API for keeping a list of servers and checking whether they are reachable. Users sign in with a JWT stored in an HTTP-only cookie, only admins can change the list or run checks, and every action is written to an audit trail in MongoDB.

## Stack

Java 21, Spring Boot 3.2.2, Spring Security 6, JJWT 0.11.5, MongoDB, springdoc-openapi 2.2.0 (Swagger UI).

## Endpoints

| Method | Path | What it does |
| --- | --- | --- |
| POST | `/api/auth/signup` | Create a user |
| POST | `/api/auth/signin` | Log in; sets the JWT cookie (valid for 24 hours) |
| POST | `/api/v1/ip_list` | Add a host |
| GET | `/api/v1/ip_list?ipId=...` | One host by id |
| GET | `/api/v1/ip_list/getByName?name=...` | Find hosts by name |
| GET | `/api/v1/ip_list/getByIPaddress?ipaddress=...` | Find hosts by IP address |
| PUT | `/api/v1/ip_list` | Update a host |
| DELETE | `/api/v1/ip_list?ipId=...` | Delete a host |
| GET | `/api/v1/ip_checking?ipId=...` | Check whether one host is reachable |
| GET | `/api/v1/ip_checking/checkall` | Check every host |

Reachability uses Java's `InetAddress.isReachable` with a one-second timeout.

## Run it

1. Start MongoDB on `localhost:27017`. The app uses a database called `demotest`.
2. Import the sample data in this folder (`demotest.roles.json`, `demotest.users.json`, `demotest.ip_list.json`, `demotest.audit_trail.json`) with MongoDB Compass or `mongoimport`. The `roles` collection must contain `ROLE_USER` and `ROLE_ADMIN` before anyone signs up.
3. Start the app:
   ```bash
   ./mvnw spring-boot:run
   ```
4. Open http://localhost:8080/swagger-ui/index.html to try the endpoints.

The JWT secret in `application.properties` is a demo value. Replace it before running this anywhere other than your own machine.
