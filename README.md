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
