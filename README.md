# Fluffy Plushies Webshop

A webshop project focused on authentication, role-based access control, and security testing. Users can browse products and write reviews. Merchants manage their own products and images, while administrators manage users and products. Products can be public or private.

Built with Angular 20, Java 24, Spring Boot 4, MySQL, RabbitMQ, and Docker Compose.

## Project structure

| Directory | Purpose |
| --- | --- |
| [Webshop](Webshop) | Angular frontend |
| [IdentityService](IdentityService) | Registration, authentication, and user management |
| [InventoryService](InventoryService) | Products, reviews, image uploads, and access control |

`OrderService` (.NET) and `EmailService` (Node.js/TypeScript) are retained from the earlier project. They are outside the current security-focused application and do not start with the default Compose configuration.

## Run locally

Requirements: Docker with Docker Compose.

1. Copy [dotenv-template](dotenv-template) to `.env` in the repository root and fill in the values, including database credentials, the JWT secret, and demo account passwords.
2. From the repository root, run:

   ```bash
   docker compose up -d
   ```

3. Open <http://localhost>.

| Service | URL |
| --- | --- |
| Webshop | <http://localhost> |
| IdentityService | <http://localhost:8081/identity> |
| InventoryService | <http://localhost:8082/inventory> |

For frontend development outside Docker, see [Webshop/README.md](Webshop/README.md).

### Demo accounts

Docker Compose enables the `demo` Spring profile, which seeds these accounts. Use the passwords configured in `.env`.

| Username | Role | Password variable |
| --- | --- | --- |
| `admin` | ADMIN | `IDENTITY_ADMIN_PASSWORD` |
| `merchant` | MERCHANT | `IDENTITY_MERCHANT_PASSWORD` |
| `user` | USER | `IDENTITY_USER_PASSWORD` |

Demo deployment: <https://shop.starlords.eu/>. This was provided for project evaluation, so continued availability is not guaranteed.

### Reset local data

The following removes the Compose volumes and their stored data, then rebuilds and starts the application:

```bash
docker compose down -v
docker compose up --build -d
```

## Tests and security documentation

- [Identity API tests](IdentityService/src/test/java/com/dlshomies/fluffyplushies/api/UserControllerApiTest.java)
- [Image-validation tests](InventoryService/src/test/java/com/sirmeows/fluffyinventoryservice/util/ImageSanitizationUtilTest.java)
- [Access-control matrix](Documentation/ACCESS_CONTROL_MATRIX.md)
- [Manual security checks](Documentation/ManualSecurityChecks.pdf)
- [UI test flows](Documentation/UI_TEST_FLOWS.md)

### Postman

The [API collection](Postman/Fluffy-Plushies-Webshop.postman_collection.json) covers authentication, registration, role-based access, product and review operations, image uploads, and SQL/command injection checks.

1. Import the collection into Postman.
2. Set `adminPassword`, `merchantPassword`, and `userPassword` to the corresponding demo account passwords from `.env`.
3. For upload requests, select the matching files from [Postman/files](Postman/files).
4. With the stack running, run the collection from top to bottom. The Auth folder sets the tokens needed by later requests. Service URLs default to `localhost:8081` and `localhost:8082`.

See [Postman/README.md](Postman/README.md) for credential handling.
