# Threat Model and Secure Configuration

## 1. Application Overview

This project threat-models a small task management web application.

Users can:
- Register and log in
- Create tasks
- View their tasks
- Update tasks
- Delete tasks
- Reset their password through email

The application consists of a web application, database, and an external email service.

---

## 2. Assets

The main assets that need protection are:

1. User accounts
2. User credentials and password hashes
3. Session/authentication tokens
4. User task data
5. Personal information
6. Password-reset tokens
7. Application database

---

## 3. Users and External Dependencies

### Users

- Normal users
- Administrators

### External Dependency

- Email service used for password-reset emails

---

## 4. Trust Boundaries

The main trust boundaries are:

1. Internet/User to Web Application
2. Web Application to Database
3. Web Application to External Email Service

Data crossing these boundaries must be authenticated, authorized, validated, and protected.

---

## 5. Threat Model Diagram

```text
                         INTERNET
                            |
                            |
                       HTTPS Requests
                            |
                            v
                    +---------------+
                    |     USER      |
                    |  Web Browser  |
                    +-------+-------+
                            |
                    TRUST BOUNDARY
                            |
                            v
                    +---------------+
                    |    WEB APP    |
                    |               |
                    | Login / API   |
                    | Task Manager  |
                    | Session Mgmt  |
                    +-------+-------+
                            |
                     TRUST BOUNDARY
                            |
                            v
                    +---------------+
                    |   DATABASE    |
                    |               |
                    | Users         |
                    | Password Hash |
                    | Tasks         |
                    | Sessions      |
                    +---------------+

             External Dependency
                       |
                       v
                +--------------+
                | EMAIL SERVICE|
                | Password     |
                | Reset Emails |
                +--------------+
## 6. STRIDE Threat Analysis

### S - Spoofing

An attacker may obtain or guess a user's credentials and access the victim's account.

**Risk:** Account takeover.

**Controls:**
- Strong password hashing
- Multi-factor authentication for sensitive accounts
- Login rate limiting
- Secure session management

### T - Tampering

A user may attempt to modify another user's task by changing a task ID in an API request.

**Risk:** Unauthorized modification of data.

**Controls:**
- Server-side authorization checks
- Object-level access control
- Input validation
- Parameterized database queries

### R - Repudiation

A user may deny performing an important action such as deleting a task.

**Risk:** Lack of accountability.

**Controls:**
- Security audit logs
- Record user ID, action, resource, and timestamp

### I - Information Disclosure

Sensitive user information, task data, or password-reset information may be exposed.

**Risk:** Privacy breach and account compromise.

**Controls:**
- HTTPS
- Access controls
- Database access restrictions
- Secure secret management

### D - Denial of Service

An attacker may send a large number of requests and consume application resources.

**Risk:** Application becomes unavailable.

**Controls:**
- Rate limiting
- Request size limits
- Resource limits
- Monitoring and alerting

### E - Elevation of Privilege

A normal user may attempt to access administrator functionality.

**Risk:** Unauthorized administrative access.

**Controls:**
- Server-side role validation
- Role-based access control
- Least privilege
## 7. Risk Register

| ID | Threat | STRIDE | Likelihood | Impact | Risk | Mitigation |
|---|---|---|---|---|---|---|
| R1 | Account takeover | Spoofing | High | High | Critical | MFA, strong password hashing, rate limiting |
| R2 | Access to another user's tasks | Tampering / Elevation | High | High | Critical | Server-side authorization |
| R3 | SQL injection | Tampering | Medium | High | High | Parameterized queries |
| R4 | Sensitive data exposure | Information Disclosure | Medium | High | High | HTTPS and access controls |
| R5 | Login endpoint abuse | Denial of Service | High | Medium | High | Rate limiting |
| R6 | Unauthorized admin access | Elevation | Medium | High | High | RBAC and least privilege |
| R7 | Missing security audit logs | Repudiation | Medium | Medium | Medium | Security logging |
| R8 | Password-reset token theft | Spoofing | Low | High | High | Short-lived single-use tokens |

## 8. Prioritized Hardening Checklist

### Priority 1 - Critical

- [ ] Enforce HTTPS for all application traffic
- [ ] Implement server-side authorization
- [ ] Use strong password hashing such as Argon2id or bcrypt
- [ ] Add rate limiting to authentication endpoints
- [ ] Use Secure, HttpOnly, and SameSite session cookies
- [ ] Validate all user input
- [ ] Use parameterized database queries

### Priority 2 - High

- [ ] Enable MFA for administrator and sensitive accounts
- [ ] Implement role-based access control
- [ ] Secure password-reset tokens
- [ ] Protect sensitive data
- [ ] Store secrets outside source code
- [ ] Restrict database access
- [ ] Implement security audit logging

### Priority 3 - Medium

- [ ] Add monitoring and security alerts
- [ ] Keep application dependencies updated
- [ ] Configure security HTTP headers
- [ ] Set request and upload size limits
- [ ] Maintain regular backups
- [ ] Test backup restoration

### Priority 4 - Ongoing

- [ ] Perform periodic vulnerability scanning
- [ ] Review user permissions regularly
- [ ] Review logs for suspicious activity
- [ ] Update the threat model after major changes
- [ ] Conduct periodic security reviews

## 9. Risk Prioritization

The highest priority risks are account takeover and unauthorized access to another user's data because they have both high likelihood and high impact.

The first hardening actions should focus on authentication security, server-side authorization, secure session management, input validation, safe database queries, rate limiting, and protection of sensitive data.

## 10. Conclusion

This threat model identifies the main security risks for a small task management web application.

The most important defensive controls are strong authentication, server-side authorization, secure session management, input validation, parameterized database queries, rate limiting, access control, and security logging.
