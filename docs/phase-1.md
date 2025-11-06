# Phase 1 Overview
This is where the phase 1 documentation is going to be stored assuming this project will end up being much larger than previous projects.

So far, it will be split into three main sections:
- [Project Design Notes](#project-design-notes)
- [Threat Model](#threat-model)
- [Specific Requirements](#specific-requirements)

---

# Project Design Notes
## 1. Overview

The Secure Credential Management System (SCMS) is designed to allow multiple users to securely store and manage their sensitive credentials in an encrypted database. The system emphasizes strong encryption, role-based access control (RBAC), detailed logging, and Splunk integration for real-time monitoring.

---

## 2. Objectives

- Provide a centralized, secure location for users to store credentials
- Ensure all data at rest is encrypted and passwords are properly hashed/salted
- Maintain audit logs for every access, modification, and deletion
- Send logs to Splunk for centralized security monitoring and analytics
- Support multiple user roles (e.g., Admin, Standard User)

---

## 3. Technology Stack

| Component      | Technology                    | Description                                                              |
| -------------- | ----------------------------- | ------------------------------------------------------------------------ |
| Backend        | Python (Flask or FastAPI)     | Provides REST API endpoints for CRUD operations and authentication       |
| Database       | MySQL                         | Stores encrypted credentials and user information                        |
| Encryption     | Python `cryptography` library | AES/Fernet for credential encryption; bcrypt/Argon2 for password hashing |
| Logging        | Python `logging` + Splunk HEC | Logs all user actions locally and to Splunk                              |
| Authentication | JWT or Flask-Login            | Manages user sessions securely                                           |
| OS/Platform    | Windows 11 VM / Linux VM      | Local test environment                                                   |

---

## 4. Key Design Decisions

- **Encryption at rest**: Credentials are stored encrypted in the database using a symmetric key stored in a secure key vault or environment variable
- **Password hashing**: User passwords are hashed with bcrypt/Argon2 to prevent reversible storage
- **RBAC**: Different user roles restrict what data or actions are accessible
- **Logging & Monitoring**: All major events (login, credential create/edit/delete, failed access attempts) are logged with timestamps and sent to Splunk
- **Environment Variables**: Sensitive information (database credentials, Splunk tokens, keys) are loaded via `.env`

---

## 5. Anticipated Challenges

- Securely managing encryption keys
- Ensuring minimal performance overhead from encryption/decryption
- Handling multi-user concurrency in database transactions
- Preventing log overload or sensitive data exposure in logs

---

## 6. Possible Next Steps

- Finalize schema and RBAC model
- Begin implementation of authentication system
- Configure MySQL and Splunk test environments
- Develop core CRUD operations with encryption enabled

---

# Threat Model
## 1. Overview

This document outlines potential security threats, risks, and mitigations for the SCMS, focusing on protecting user credentials, authentication data, and system integrity.

---

## 2. Assets to Protect

- User credentials (encrypted data)
- User authentication hashes
- Encryption keys
- Audit logs and Splunk data
- Application configuration and source code

---

## 3. Identified Threats and Mitigations

| Threat                            | Description                                         | Impact   | Mitigation                                                                                |
| --------------------------------- | --------------------------------------------------- | -------- | ----------------------------------------------------------------------------------------- |
| **SQL Injection**                 | Malicious SQL statements could expose data.         | High     | Use parameterized queries and ORM tools.                                                  |
| **Brute-force login**             | Repeated login attempts to guess passwords.         | Medium   | Rate-limit requests; account lockout policy.                                              |
| **Privilege escalation**          | Regular users attempt to gain admin rights.         | High     | Implement strict RBAC and validate permissions on every request.                          |
| **Data leakage through logs**     | Sensitive data accidentally stored in logs.         | Medium   | Mask sensitive fields; sanitize log data.                                                 |
| **Key compromise**                | Encryption key exposure through filesystem or logs. | Critical | Store keys securely in environment variables or external vault; rotate keys periodically. |
| **Unsecured Splunk transmission** | Logs intercepted during network transfer.           | Medium   | Use HTTPS for Splunk HEC endpoint; verify SSL certs.                                      |
| **Session hijacking**             | Attacker reuses stolen JWT or cookies.              | High     | Use short-lived tokens and enforce HTTPS-only cookies.                                    |

---

## 4. Attack Surface Summary

| Category | Entry Points                            |
| -------- | --------------------------------------- |
| Network  | API endpoints (login, credential CRUD)  |
| System   | Database access, Splunk HEC credentials |
| User     | Weak passwords, phishing attempts       |
| Logging  | Excessive or verbose logs leaking data  |

---

## 5. Security Controls Summary

- **Authentication**: Strong password policy, hashing, and session management
- **Encryption**: AES/Fernet for credential data, bcrypt/Argon2 for password hashing
- **Logging**: Centralized structured logs, sanitized before sending to Splunk
- **Access Control**: Role validation enforced on every endpoint
- **Monitoring**: Splunk dashboards for anomaly detection

---

# Specific Requirements
## Overview
These are the goal requirements for the project and may end up being altered by the end of my work. I'm creating this as I go so I chose to give myself a sort of baseline to strive for.

---

## 1. Functional Requirements

### 1.1 User Management

- The system must allow new users to register with a unique username and secure password
- User passwords must be hashed (bcrypt or Argon2) before storage
- The system must support at least two roles: **Admin** and **User**

### 1.2 Credential Management

- Users can create, read, update, and delete their own credentials
- All stored credentials must be encrypted using AES/Fernet
- Admin users can audit all credentials without viewing plaintext values

### 1.3 Logging & Monitoring

- All key actions (login, logout, credential creation/modification/deletion, failed attempts) must be logged
- Logs must include user ID, timestamp, action type, and outcome
- Logs must be sent securely to Splunk via HTTP Event Collector (HEC)

### 1.4 Authentication & Session Handling

- The system must use JWT or session tokens for authentication
- Tokens must expire within a defined window (e.g., 30 min)
- Users must reauthenticate after token expiration

---

## 2. Non-Functional Requirements

| Category            | Requirement                                                         |
| ------------------- | ------------------------------------------------------------------- |
| **Security**        | Encryption at rest, hashed passwords, HTTPS communication           |
| **Availability**    | Support 10+ concurrent users                                        |
| **Usability**       | Provide CLI or minimal web interface for credential access          |
| **Maintainability** | Code modularized into distinct components (auth, db, logging)       |
| **Scalability**     | System should scale to additional users with minimal config changes |
| **Compliance**      | Follow OWASP Top 10 secure coding practices                         |

---

## 3. Constraints

- All secrets (DB password, encryption key, Splunk token) must be managed via environment variables
- The project will initially deploy locally, not on a public network
- Python 3.11+ and MySQL 8.x must be used

---

## 4. Possible Future Enhancements

- Integration with a hardware security module (HSM) for key storage
- Optional web-based dashboard for users
- Multi-factor authentication (MFA) for login
- Role-based dashboards in Splunk for user activity analytics

