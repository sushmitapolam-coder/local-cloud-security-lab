# Local Cloud Security Lab

A beginner cloud-security project that simulates secure S3-compatible object storage locally using Docker, SeaweedFS, AWS CLI, and Nginx.

## Project Objective

The goal of this project is to understand fundamental cloud-security concepts without requiring a paid cloud account.

The lab demonstrates:

- Authentication
- Authorization
- Least-privilege access
- S3-compatible object storage
- Role-based permissions
- Audit logging
- HTTPS/TLS
- Encryption at rest
- Encryption in transit

## Architecture

AWS CLI
   |
   | HTTPS / TLS
   v
Nginx Audit Proxy
   |
   | Request Logging
   v
SeaweedFS S3 Storage
   |
   +---------------------+
   |                     |
   v                     v
 Admin                Student
 Read                  Read
 Write                 List
 Delete                No Write
                       No Delete

## Technologies Used

- Docker Desktop
- SeaweedFS
- AWS CLI
- Nginx
- OpenSSL
- PowerShell

## Access-Control Model

### Admin

The admin user can:

- List objects
- Read objects
- Upload objects
- Delete objects
- Perform administrative operations

### Student

The student user follows the principle of least privilege.

The student can:

- List objects
- Download/read objects

The student cannot:

- Upload objects
- Delete objects

Unauthorized write attempts return:

AccessDenied

## Authentication Testing

The lab verifies that invalid AWS-style credentials are rejected.

Example:

InvalidAccessKeyId

This demonstrates that the S3 endpoint requires valid credentials.

## Audit Logging

Nginx is used as a reverse proxy in front of the S3 service.

Successful requests are logged with HTTP status codes such as:

GET -> 200

Unauthorized operations are logged as:

PUT -> 403

This demonstrates how cloud environments can record successful and failed access attempts.

## Encryption in Transit

The Nginx proxy uses HTTPS with TLS.

The AWS CLI communicates with the storage service through:

https://localhost:8443

This protects data while it travels between the client and storage service.

## Encryption at Rest

Sensitive files are encrypted using AES-256 before being uploaded.

Flow:

Plaintext file
   |
AES-256 Encryption
   |
Encrypted object
   |
S3-compatible storage

The encrypted object cannot be meaningfully read without the correct encryption secret.

## Security Concepts Learned

This project demonstrates several core cloud-security principles:

1. Authentication - verifying user identity.
2. Authorization - controlling what authenticated users can do.
3. Least Privilege - granting only required permissions.
4. Encryption in Transit - protecting data using TLS.
5. Encryption at Rest - protecting stored data with encryption.
6. Audit Logging - recording successful and failed access attempts.
7. Credential Validation - rejecting unknown access keys.

## Example Security Test

Student attempts to upload an object:

PUT /student-secure-files/file.txt

Result:

403 Access Denied

Student downloads an existing object:

GET /student-secure-files/file.txt

Result:

200 OK

## Disclaimer

This project is designed for local learning and demonstration purposes.

Production cloud environments should use managed secret storage, certificate authorities, centralized logging, key-management systems, and properly secured infrastructure.
