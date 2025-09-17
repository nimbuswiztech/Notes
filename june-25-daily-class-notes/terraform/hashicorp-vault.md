# Hashicorp vault

## HashiCorp Vault Demo Project: Complete Setup and Implementation Guide <a href="#hashicorp-vault-demo-project-complete-setup-and-im" id="hashicorp-vault-demo-project-complete-setup-and-im"></a>

This comprehensive guide provides everything you need to demonstrate HashiCorp Vault functionality, including a complete project structure with Docker configuration, automation scripts, and practical examples for secrets management.

### What is HashiCorp Vault? <a href="#what-is-hashicorp-vault" id="what-is-hashicorp-vault"></a>

HashiCorp Vault is a powerful tool for **securely storing and managing sensitive data** such as API keys, passwords, certificates, database credentials, and encryption keys. Vault provides a centralized location for storing secrets with robust security features including encryption, access control, and comprehensive audit logging. Unlike traditional static secret storage, Vault offers **dynamic secrets generation**, where credentials are created on-demand and automatically expire, significantly reducing security risks.

### Key Features and Benefits <a href="#key-features-and-benefits" id="key-features-and-benefits"></a>

**Centralized Secret Management**: Vault provides a single, secure location to store all sensitive data, eliminating the need to hardcode secrets in applications or configuration files. This centralized approach allows organizations to manage and rotate secrets without updating every application that uses them.

**Dynamic Secrets**: One of Vault's most powerful features is its ability to generate **temporary, short-lived credentials** on demand. For example, when an application needs database access, Vault can generate unique database credentials that expire automatically after a specified time period.

**Encryption as a Service**: Vault offers robust encryption capabilities using industry-standard algorithms to encrypt data both at rest and in transit. This ensures that even if someone gains access to the storage backend, the data remains encrypted and secure.

**Fine-Grained Access Control**: Vault implements path-based policies that define specific rules restricting actions and access to designated paths for each client. This allows administrators to create detailed access controls ensuring only authorized users can access specific secrets.

**Comprehensive Audit Logging**: Every interaction with Vault is logged, providing detailed audit trails that track who accessed what secrets and when. This helps organizations comply with regulatory requirements and investigate potential security incidents.

HashiCorp Vault demo project architecture diagram

### Demo Project Structure <a href="#demo-project-structure" id="demo-project-structure"></a>

The provided demo project includes a complete directory structure designed for hands-on learning and demonstration:

### Core Components

**Docker Configuration**: The project uses Docker Compose to orchestrate Vault server and a PostgreSQL database for demonstrating dynamic secrets functionality. The `docker-compose.yml` file configures Vault with proper security settings including the IPC\_LOCK capability to prevent sensitive data from being swapped to disk.

**Vault Configuration Files**: Two configuration files are provided - `vault.hcl` for production-like setup using Raft storage backend, and `vault-dev.hcl` for development mode with in-memory storage. The production configuration includes UI enablement, storage backend specification, and API listener configuration.

**Automation Scripts**: Four shell scripts automate common Vault operations:

* `basic-demo.sh`: Demonstrates KV secrets engine operations
* `database-demo.sh`: Shows dynamic database credential generation
* `setup-policies.sh`: Configures access control policies and user authentication
* `cleanup.sh`: Provides environment cleanup functionality

**Access Control Policies**: Three HCL policy files demonstrate different access levels:

* `app-read.hcl`: Read-only access for applications
* `app-write.hcl`: Read-write access for development
* `db-admin.hcl`: Administrative access for database management

### Programming Examples

**Python Integration**: The project includes a complete Python client using the `hvac` library, demonstrating how to authenticate, read/write secrets, and generate dynamic database credentials programmatically. This shows real-world application integration patterns.

**API Examples**: Shell scripts using `curl` demonstrate direct REST API interaction with Vault, showing how to perform operations without language-specific libraries.

### Step-by-Step Implementation <a href="#step-by-step-implementation" id="step-by-step-implementation"></a>

### Phase 1: Environment Setup

**1. Extract and Navigate**

```
unzip hashicorp-vault-demo-project.zip
cd vault-demo-project
```

**2. Start Services**

```
# Start Vault and PostgreSQL containers
docker-compose up -d

# Verify services are running
docker-compose ps
```

**3. Initialize Vault** (First time only)

```
# Set environment variable
export VAULT_ADDR='http://localhost:8200'

# Initialize Vault and save output
docker exec vault-server vault operator init
```

The initialization process generates **5 unseal keys and 1 root token**. Save these securely as they're required for unsealing and administrative access.

### Phase 2: Vault Unsealing

**4. Unseal Vault**

```
# Use any 3 of the 5 unseal keys
docker exec vault-server vault operator unseal <key1>
docker exec vault-server vault operator unseal <key2>
docker exec vault-server vault operator unseal <key3>
```

**5. Authenticate**

```
# Set root token from initialization
export VAULT_TOKEN=<root_token>

# Verify authentication
docker exec vault-server vault auth <root_token>
```

### Phase 3: Basic Secrets Management

**6. Run Basic Demo**

```
# Make scripts executable
chmod +x scripts/*.sh

# Execute basic secrets demo
./scripts/basic-demo.sh
```

This script demonstrates:

* Creating secrets in the KV store using `vault kv put`
* Reading secrets with `vault kv get`
* Managing versioned secrets with the KV v2 engine
* Listing available secrets with `vault kv list`

### Phase 4: Dynamic Secrets

**7. Database Secrets Demo**

```
./scripts/database-demo.sh
```

This demonstrates the **database secrets engine** which:

* Enables the database plugin for PostgreSQL
* Configures connection parameters to the demo database
* Creates roles defining credential creation patterns
* Generates dynamic, temporary database credentials on demand

### Phase 5: Access Control

**8. Setup Policies and Authentication**

```
./scripts/setup-policies.sh
```

This script:

* Creates three different access policies with varying permission levels
* Enables **userpass authentication method**
* Creates users with specific policy assignments
* Demonstrates role-based access control

### Phase 6: Programming Integration

**9. Python Client Example**

```
cd examples/python
pip install -r requirements.txt
python vault_client.py
```

**10. API Integration**

```
cd examples/curl
export VAULT_TOKEN=<your_token>
./api-examples.sh
```

### Advanced Configuration Options <a href="#advanced-configuration-options" id="advanced-configuration-options"></a>

### Production Considerations

**Storage Backend**: The demo uses Raft storage for persistence. In production, consider:

* **Consul** for high availability clustering
* **Cloud storage** backends for managed infrastructure
* **Database backends** for existing infrastructure integration

**TLS Configuration**: Production deployments must enable TLS encryption. The demo disables TLS for simplicity, but production requires proper certificate management.

**High Availability**: Production Vault should run in **cluster mode** with multiple nodes for fault tolerance. The demo provides a single-node setup suitable for learning and development.

**Seal Methods**: While the demo uses **Shamir's Secret Sharing** with 5 keys requiring 3 for unsealing, production often uses **auto-unseal** with cloud KMS services for operational efficiency.

### Security Best Practices

**Token Management**: Implement **short-lived tokens** with appropriate TTL values. The demo includes examples of token creation with specific policies and time limitations.

**Policy Design**: Follow the **principle of least privilege** when designing access policies. The demo policies demonstrate different access levels from read-only to administrative access.

**Audit Logging**: Enable comprehensive audit logging in production to track all Vault operations. This is crucial for security monitoring and compliance requirements.

**Secret Rotation**: Implement automated secret rotation policies. The database secrets engine in the demo automatically handles credential lifecycle management.

### Troubleshooting Common Issues <a href="#troubleshooting-common-issues" id="troubleshooting-common-issues"></a>

**Vault Sealed**: If Vault becomes sealed, use the unseal keys from initialization. This is a normal security measure that can occur after restarts or in certain error conditions.

**Permission Denied**: Ensure your token has appropriate policies attached. The demo includes examples of policy-based access control that can help diagnose permission issues.

**Connection Issues**: Verify the `VAULT_ADDR` environment variable points to the correct Vault instance. The demo uses `http://localhost:8200` for local development.

**Docker Issues**: Ensure Docker has sufficient resources and the IPC\_LOCK capability is properly configured. This prevents sensitive data from being swapped to disk.

### Cleanup and Maintenance <a href="#cleanup-and-maintenance" id="cleanup-and-maintenance"></a>

When finished with the demo:

```
# Stop and clean up all resources
./scripts/cleanup.sh

# Or manually
docker-compose down
docker volume prune -f
```

This comprehensive demo project provides a complete learning environment for HashiCorp Vault, covering everything from basic setup to advanced features like dynamic secrets and access control policies. The included documentation, scripts, and examples offer multiple learning paths suitable for different experience levels and use cases.

### Real-time use case

A real-time use case for HashiCorp Vault is securing database credentials for microservices deployed in a Kubernetes cluster (such as AWS EKS). Instead of hardcoding static database usernames and passwords within application configurations or environment variables, Vault dynamically generates short-lived database credentials when a microservice requests access. These credentials are automatically revoked after a configured time-to-live, minimizing the risk if credentials are compromised.

In this setup, Vault is integrated with Kubernetes via a sidecar injector or CSI driver that injects secrets into pods at runtime. When a microservice starts, it authenticates to Vault using its Kubernetes service account token, then retrieves its unique database credentials from Vault’s database secrets engine. The application uses these credentials to connect to a managed database (e.g., Amazon RDS or PostgreSQL). Vault handles periodic credential rotation transparently, enabling secure secret lifecycle management without manual intervention.

This approach provides:

* **Improved security** by eliminating long-lived static credentials
* **Automated credential lifecycle** with dynamic creation and revocation
* **Centralized access control and audit logging** for compliance
* **Seamless Kubernetes integration**, enabling secret injection without application code changes

Such real-time Vault usage protects sensitive access keys across dynamic cloud-native environments, improving overall infrastructure security in production microservices deployments.
