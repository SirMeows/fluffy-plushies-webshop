# Fluffy Plushies Webshop Frontend

Angular 20 and TypeScript frontend for the [Fluffy Plushies Webshop](../README.md). It provides registration and login, product browsing, reviews, and role-based product and user management.

## Run the full application

Follow the [Docker Compose setup](../README.md#run-locally) in the main README. It builds the frontend and starts the required backend services.

## Frontend development

Requirements: Node.js 22.12 or newer within the 22.x release line, npm, and the backend stack running as described in the main README.

From the `Webshop` directory:

```bash
npm ci
npm start
```

Open <http://localhost:4200>. The development server reloads when source files change.

[proxy.conf.json](proxy.conf.json) forwards `/identity` to `http://localhost:8081` and `/inventory` to `http://localhost:8082`. The frontend uses these relative API paths in [environment.ts](src/environments/environment.ts).

## Build and tests

Run these commands from `Webshop`:

```bash
npm run build
npm test
```

The production build is written to `dist/fluffy_plushies_web_app/browser`. Unit tests use Jasmine and Karma with a Chrome browser. See the [UI test flows](../Documentation/UI_TEST_FLOWS.md) for manual checks.
