# Safe Web Application Security Review Checklist

Use this checklist for authorized defensive reviews of your own web application code. It is intentionally focused on prevention, code review, configuration review, and safe verification. It does not include exploit weaponization or attack steps.

This checklist is not permission to test systems you do not own or are not explicitly authorized to review.

## How to Use This Checklist

- Define the application, repositories, environments, and accounts that are in scope before reviewing.
- Review code and configuration in a non-production environment whenever possible.
- Record findings with file paths, configuration names, impact, owner, and remediation status.
- Prefer automated checks for repeatability, but manually review security-sensitive flows.
- Treat secrets, customer data, logs, and screenshots as sensitive review artifacts.

## 1. Exposed Secrets and Sensitive Configuration

- [ ] Confirm no API keys, tokens, passwords, private keys, session secrets, or database URLs are committed to the repository.
- [ ] Check examples, tests, fixtures, Docker files, CI files, deployment manifests, and documentation for real credentials.
- [ ] Ensure `.env`, local config files, build outputs, logs, and credential files are ignored by version control.
- [ ] Verify secrets are loaded from a managed secret store or protected environment variables, not hard-coded constants.
- [ ] Confirm production, staging, and development secrets are different.
- [ ] Rotate any secret that was committed, logged, shared in chat, or exposed in build artifacts.
- [ ] Avoid printing tokens, authorization headers, cookies, reset links, or personally identifiable information in logs.
- [ ] Confirm error reports and telemetry redact sensitive values before leaving the application environment.

## 2. Authentication and Session Management

- [ ] Verify every protected route requires authentication on both the frontend and backend.
- [ ] Confirm backend authorization does not rely only on hidden UI elements or client-side checks.
- [ ] Ensure password storage uses a strong, purpose-built password hashing algorithm with appropriate parameters.
- [ ] Require secure password reset flows with short-lived, single-use reset tokens.
- [ ] Invalidate sessions after logout, password change, account deletion, and suspicious account activity.
- [ ] Set cookies with `HttpOnly`, `Secure`, and appropriate `SameSite` attributes.
- [ ] Use short-lived access tokens and safe refresh-token rotation where token-based authentication is used.
- [ ] Protect multi-factor authentication enrollment, removal, and recovery flows from unauthorized changes.
- [ ] Apply rate limiting and abuse monitoring to login, registration, password reset, and MFA verification endpoints.
- [ ] Review account enumeration risks in login and password reset responses.

## 3. Authorization and Access Control

- [ ] Check that every API endpoint enforces authorization server-side.
- [ ] Verify users can only access resources that belong to them or their permitted organization, role, or tenant.
- [ ] Review admin, billing, export, invitation, impersonation, and role-management features carefully.
- [ ] Use deny-by-default authorization for new routes and actions.
- [ ] Ensure object identifiers in URLs or request bodies are validated against the authenticated user’s permissions.
- [ ] Confirm background jobs, webhooks, GraphQL resolvers, file downloads, and batch actions enforce the same permissions as normal requests.
- [ ] Test role transitions, disabled accounts, deleted resources, and pending invitations for stale access.

## 4. Security Headers and Browser Protections

- [ ] Set a restrictive `Content-Security-Policy` that matches the application’s actual script, style, image, and connection needs.
- [ ] Enable `Strict-Transport-Security` for HTTPS-only production domains.
- [ ] Set `X-Content-Type-Options: nosniff`.
- [ ] Use `Referrer-Policy` to limit unnecessary URL leakage.
- [ ] Use `Permissions-Policy` to disable browser features the application does not need.
- [ ] Prevent clickjacking with `frame-ancestors` in CSP or an appropriate frame policy.
- [ ] Avoid exposing framework, server, or infrastructure version details in response headers.
- [ ] Confirm CORS allows only trusted origins, required methods, and required headers.

## 5. Input Validation, Output Encoding, and Injection Prevention

- [ ] Validate request bodies, query strings, route parameters, headers, uploaded files, and webhook payloads at trust boundaries.
- [ ] Use allowlists for enums, identifiers, sort fields, redirect targets, MIME types, and file extensions.
- [ ] Use parameterized database queries or safe ORM APIs instead of string-building queries.
- [ ] Escape or encode untrusted content before rendering it in HTML, JavaScript, CSS, URLs, Markdown, emails, or templates.
- [ ] Sanitize rich text or HTML using a maintained sanitizer configured for the minimum necessary tags and attributes.
- [ ] Validate numeric limits such as page size, quantity, price, discount, retry count, and timeout values.
- [ ] Reject unexpected fields to prevent mass-assignment or privilege changes through hidden request properties.
- [ ] Normalize and validate paths before file reads, writes, imports, or archive extraction.

## 6. Unsafe API Behavior

- [ ] Require authentication and authorization for all non-public API operations.
- [ ] Enforce consistent request size, pagination, rate, and timeout limits.
- [ ] Return minimal error messages to clients while logging safe diagnostic details internally.
- [ ] Avoid returning secrets, internal IDs, debug traces, stack traces, or unnecessary user fields.
- [ ] Validate state-changing requests against CSRF where browser cookies are used for authentication.
- [ ] Require idempotency keys for payment, order, or other high-impact retryable operations.
- [ ] Verify webhook handlers authenticate the sender, validate payload shape, and handle replay safely.
- [ ] Protect file upload, import, export, and report-generation endpoints with strict validation and resource limits.
- [ ] Confirm public APIs have documented behavior for authentication, authorization, error handling, and rate limits.

## 7. Dependencies and Supply Chain

- [ ] Keep package managers, lockfiles, runtime versions, and base images current.
- [ ] Run dependency vulnerability scanning in CI and review high-impact findings promptly.
- [ ] Remove unused dependencies, abandoned packages, and packages with unnecessary install scripts.
- [ ] Pin or lock dependency versions for reproducible builds.
- [ ] Review new dependencies for maintainer reputation, update history, license, and transitive risk.
- [ ] Build from trusted registries and verify container images come from trusted sources.

## 8. Data Protection and Privacy

- [ ] Classify sensitive data handled by the application and document where it is stored, processed, logged, and transmitted.
- [ ] Use TLS for data in transit and managed encryption for sensitive data at rest.
- [ ] Store only the data needed for the product and retention requirements.
- [ ] Mask sensitive values in admin views, support tools, logs, analytics, and exports.
- [ ] Protect backups, data exports, and generated reports with access controls and retention limits.
- [ ] Verify deletion, anonymization, and account-closure flows behave as documented.

## 9. Logging, Monitoring, and Incident Readiness

- [ ] Log security-relevant events such as login attempts, permission changes, token rotation, password resets, admin actions, and data exports.
- [ ] Include enough context for investigation without storing secrets or excessive personal data.
- [ ] Alert on unusual authentication failures, privilege changes, high error rates, and suspicious API usage patterns.
- [ ] Ensure logs are protected from unauthorized access and tampering.
- [ ] Document who receives security alerts and how incidents are triaged.
- [ ] Maintain a tested process for rotating secrets and disabling compromised accounts or tokens.

## 10. Deployment and Environment Hardening

- [ ] Disable debug mode, development error pages, test routes, seed data, and sample accounts in production.
- [ ] Use least-privilege permissions for application roles, service accounts, database users, and CI/CD tokens.
- [ ] Separate production, staging, and development environments and data.
- [ ] Protect CI/CD variables, deployment keys, signing keys, and package publishing tokens.
- [ ] Require review for infrastructure, permission, secret, and deployment workflow changes.
- [ ] Verify backups, migrations, rollbacks, and disaster recovery procedures are documented and periodically tested.

## Safe Review Evidence Template

For each issue, record:

- **Finding:** Short description of the security concern.
- **Location:** Repository path, configuration name, route, or component.
- **Risk:** What could go wrong if left unfixed.
- **Safe verification:** Code/configuration evidence or non-invasive test result.
- **Remediation:** Specific defensive change to make.
- **Owner and due date:** Person/team responsible and expected completion date.
- **Status:** Open, in progress, fixed, accepted risk, or not applicable.
