# Secure Credential Management System

## Objective

This project will be focusing on creating a secure credential management system. It will mainly be focused on both security and funcitonality with less emphasis on the actual user interface. My original plan was to create a password manager but I think it aligns more with a secure credential management system. One main goal of this project is to improve/showcase my security engineering skillset.

### Skills Learned/Improved

- Placeholder skill

### Tools/Technologies Used

- Placeholder tool/tech

# Steps

## Initial Roadmap
- This is the prospective roadmap for the project that may change by the end
- To quickly list the basic functions/capabilities I wanted the system to have:
	- Code written in python for the functionality
	- Connected to a MySQL database which would encrypt the data at rest
	- All passwords would be hashed and salted
	- Create a logging system for each time an account is created, edited, or viewed
	- Send logs to Splunk
- Now moving into the actual roadmap for the project, the sections are split into phases with each phase containing a goal, tasks, and deliverables

### Phase 1 – Planning & Architecture
- Goal: Establish the system’s foundation, security model, and design
- Tasks:
	- Define system purpose and boundaries
	- Draft data flow diagrams
	- Design database schema:
		- `users` table for authentication
		- `credentials` table for encrypted data
		- `logs` table for audit events
	- Select security libraries
	- Plan key management strategy
	- Document logging requirements for Splunk ingestion
- Deliverables:
	- Architecture diagram
	- Data model design
	- Security and logging specifications

### Phase 2 – Backend Core Development
- Goal: Build the foundational backend services for credential management
- Tasks:
	- Set up Python environment and MySQL connection pool
	- Implement secure user authentication system
	- Implement Create, Read, Update, and Delete (CRUD) operations for credentials:
		- Encrypt credentials before storing them
		- Decrypt only when authorized
	- Apply password hashing and salting for all user passwords
	- Build error handling and input validation layers
- Deliverables:
	- Functional Python scripts for user and credential management
	- MySQL schema with sample data
	- Encrypted credential storage verified

### Phase 3 – Encryption & Data Protection Layer
- Goal: Ensure data confidentiality, integrity, and secure key handling
- Tasks:
	- Implement encryption for credential fields
	- Separate encryption keys from database
	- Introduce optional key rotation mechanism for long-term security
	- Verify encryption-at-rest compliance
	- Document encryption and decryption flow for internal use
- Deliverables:
	- Working encryption module
	- Key management documentation
	- Encryption test results

### Phase 4 – Logging & Auditing System
- Goal: Enable full visibility and traceability of all system actions
- Tasks:
	- Create an internal logging system
	- Log all events:
		- Account creation, modification, and deletion
		- Credential access (read/view) attempts
		- Failed logins or suspicious activity
	- Implement log rotation and file integrity checks
	- Build a Splunk forwarder or possibly use the Splunk HTTP Event Collector (HEC) for real-time ingestion
	- Design Splunk dashboards:
		- User activity overview
		- Access frequency trends
		- Alerting rules for anomalies
- Deliverables:
	- Local logging module
	- Splunk integration working
	- Basic Splunk dashboard and alerts

### Phase 5 – Security Enhancements & Access Control
- Goal: Strengthen authentication, access control, and resilience
- Tasks:
	- Add role-based access control (RBAC) — e.g., Admin vs Standard User
	- Implement Multi-Factor Authentication (MFA) using a TOTP library
	- Add session management
	- Perform input sanitization to prevent SQL injection and command injection
	- Integrate brute-force protection
- Deliverables:
	- Access control policy documentation
	- MFA and session management features
	- Verified SQL injection protection

### Phase 6 – Testing, Validation, and Splunk Tuning
- Goal: Verify system functionality and security compliance
- Tasks:
	- Conduct unit tests for each module
	- Perform penetration testing on authentication and API endpoints
	- Review Splunk logs for completeness and accuracy
	- Simulate attack scenarios
	- Optimize Splunk dashboards for real-time insights
- Deliverables:
	- Test reports
	- Splunk visualizations for monitoring credential usage
	- Final hardening checklist

### Phase 7 – Documentation & Deployment
- Goal: Finalize project deliverables, usability, and reproducibility
- Tasks:
	- Write developer documentation
	- Create user/admin guide for safe credential management
	- Implement secure deployment setup:
		- Environment variables for secrets
		- Dockerfile or virtual environment for portability
	- Package final system and dashboards as a deliverable
- Deliverables:
	- Full project documentation
	- Deployment package or repository
	- Final demo or presentation-ready system

- Any specific products/utilities mentioned above are subject to change as I begin working on each phase of the project
- These are from initial research and by no means are definitively being used

---
## ⚠️ Security Disclaimer
This repository is for educational and demonstration purposes only.
All future credentials, tokens, and configurations are placeholders.
Do **not** use any future code in production without additional hardening.
