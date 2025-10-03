---
description: Learn how to set up app-level access control for your Flowise instances
---

# Application

***

## Email & Password

From v3.0.1 onwards, a new authentication method was introduced. Flowise uses a [**Passport.js**](https://www.passportjs.org/)**-based authentication system** with JWT tokens stored in secure HTTP-only cookies. When a user logs in, the system validates their email/password against the database using bcrypt hash comparison, then generates two JWT tokens: a short-lived access token (default 60 minutes) and a long-lived refresh token (default 90 days). These tokens are stored as secure cookies. For subsequent requests, the system extracts the JWT from cookies, validates the signature and claims using Passport's JWT strategy, and checks that the user session still exists. The system also supports automatic token refresh when the access token expires, maintains sessions using either Redis or database storage depending on configuration.

For existing users who have been using [Username & Password (Deprecated)](app-level.md#username-and-password-deprecated), you need to set up a new admin account. To prevent unauthorized ownership claims, you must first authenticate using the existing username and password configured as `FLOWISE_USERNAME` and `FLOWISE_PASSWORD`.

<figure><img src="../../.gitbook/assets/image (18) (1) (1).png" alt="" width="387"><figcaption></figcaption></figure>

The following environment variables can be altered:

### Application URL

* `APP_URL` - Your hosted Flowise appication URL. Default to `http://localhost:3000`

### JWT Environment Variables Configuration

To configure Flowise's JWT authentication parameters, user may alter the following environment variables:

* `JWT_AUTH_TOKEN_SECRET` - The secret key for signing access tokens
* `JWT_REFRESH_TOKEN_SECRET` - Secret for refresh tokens (defaults to auth token secret if not set)
* `JWT_TOKEN_EXPIRY_IN_MINUTES` - Access token lifetime (default: 60 minutes)
* `JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES` - Refresh token lifetime (default: 129,600 minutes or 90 days)
* `JWT_AUDIENCE` - Token validation audience claim (default: 'AUDIENCE')
* `JWT_ISSUER` - Token validation issuer claim (default: 'ISSUER')
* `EXPRESS_SESSION_SECRET` - Session encryption secret (default: 'flowise')
* `EXPIRE_AUTH_TOKENS_ON_RESTART` - Set to 'true' to invalidate all tokens on server restart (useful for development)

### SMTP Email Configuration

Configure these variables to enable email functionality for password resets, and notifications:

* `SMTP_HOST` - The hostname of your SMTP server (e.g., `smtp.gmail.com`, `smtp.host.com`)
* `SMTP_PORT` - The port number for SMTP connection (common values: `587` for TLS, `465` for SSL, `25` for unencrypted)
* `SMTP_USER` - Username for SMTP authentication (usually your email address)
* `SMTP_PASSWORD` - Password or app-specific password for SMTP authentication
* `SMTP_SECURE` - Set to `true` for SSL/TLS encryption, `false` for unencrypted connections
* `ALLOW_UNAUTHORIZED_CERTS` - Set to `true` to allow self-signed certificates (not recommended for production)
* `SENDER_EMAIL` - The "from" email address that will appear on outgoing emails

### Security and Token Configuration

These variables control authentication security, token expiration, and password hashing:

* `PASSWORD_RESET_TOKEN_EXPIRY_IN_MINS` - Expiration time for password reset tokens (default: 15 minutes)
* `PASSWORD_SALT_HASH_ROUNDS` - Number of bcrypt salt rounds for password hashing (default: 10, higher = more secure but slower)
* `TOKEN_HASH_SECRET` - Secret key used for hashing tokens and sensitive data (use a strong, random string)

### Security Best Practices

* Use strong, unique values for `TOKEN_HASH_SECRET` and store them securely
* For production, use `SMTP_SECURE=true` and `ALLOW_UNAUTHORIZED_CERTS=false`
* Set appropriate token expiry times based on your security requirements
* Use higher `PASSWORD_SALT_HASH_ROUNDS` values (12-15) for better security in production

## Username & Password (Deprecated)

App level authorization protects your Flowise instance by username and password. This protects your apps from being accessible by anyone when deployed online.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### How to Set Username & Password

#### Npm

1. Install Flowise

```bash
npm install -g flowise
```

2. Start Flowise with username & password

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

3. Open [http://localhost:3000](http://localhost:3000)

#### Docker

1. Navigate to `docker` folder

```
cd docker
```

2. Create `.env` file and specify the `PORT`, `FLOWISE_USERNAME`, and `FLOWISE_PASSWORD`

```sh
PORT=3000
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

3. Pass `FLOWISE_USERNAME` and `FLOWISE_PASSWORD` to the `docker-compose.yml` file:

```
environment:
    - PORT=${PORT}
    - FLOWISE_USERNAME=${FLOWISE_USERNAME}
    - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
```

4. `docker compose up -d`
5. Open [http://localhost:3000](http://localhost:3000)
6. You can bring the containers down by `docker compose stop`

#### Git clone

To enable app level authentication, add `FLOWISE_USERNAME` and `FLOWISE_PASSWORD` to the `.env` file in `packages/server`:

```
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```
