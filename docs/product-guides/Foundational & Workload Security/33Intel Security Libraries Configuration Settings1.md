# Verification Service 
--------------------

## Installation Answer File Options

```yaml
# Authentication URL and service account credentials - mandatory
AAS_API_URL=https://isecl-aas:8444/aas/v1
HVS_SERVICE_USERNAME=HVS_service
HVS_SERVICE_PASSWORD=password

# CMS URL and CMS webserivce TLS hash for server verification - mandatory
CMS_BASE_URL=https://isecl-cms:8445/cms/v1
CMS_TLS_CERT_SHA384=digest

# Installation admin bearer token for CSR approval request to CMS - mandatory
BEARER_TOKEN=eyJhbGciOiJSUzM4NCIsImtpZCI6ImE…

# Skip setup - optional
HVS_NOSETUP=false #default=false

# Logging options - optional
HVS_LOGLEVEL=info              # options: critical|error|warning|info|debug|trace, default='info'
HVS_LOG_MAX_LENGTH=300         # default=300
HVS_ENABLE_CONSOLE_LOG=false   # default=false

# HRRS configuration - optional
HRRS_REFRESH_PERIOD=2m0s       # default=2m0s
HRRS_REFRESH_LOOK_AHEAD=5m0s   # default=5m0s

# FVS configuration - optional
FVS_NUMBER_OF_VERIFIERS=20                    # default=20
FVS_NUMBER_OF_DATA_FETCHERS=20                # default=20
FVS_SKIP_FLAVOR_SIGNATURE_VERIFICATION=false  # default=false

# In case of trusted flavor storage, flavor signature verification can be skipped
# using following flag - optional
SKIP_FLAVOR_SIGNATURE_VERIFICATION=false # default=false

# TLS certificate configuration - optional
TLS_COMMON_NAME="HVS TLS Certificate"      # default="HVS TLS Certificate"
TLS_SAN_LIST=127.0.0.1,localhost           # default=127.0.0.1,localhost

# Server configuration - optional
HVS_PORT=8443                        # default=8443
HVS_SERVER_READ_TIMEOUT=30s          # default=30s
HVS_SERVER_READ_HEADER_TIMEOUT=10s   # default=10s
HVS_SERVER_WRITE_TIMEOUT=10s         # default=10s
HVS_SERVER_IDLE_TIMEOUT=10s          # default=10s
HVS_SERVER_MAX_HEADER_BYTES=1048576  # default=1048576

# Database - mandatory
HVS_DB_USERNAME=runner
HVS_DB_PASSWORD=test
HVS_DB_SSLCERTSRC=/tmp/dbcert.pem          # This doesn't need to be specified if HVS_DB_SSLCERT is given

# Database - optional
HVS_DB_HOSTNAME=localhost                  # default=localhost
HVS_DB_NAME=hvs-pg-db                      # default=hvs-pg-db
HVS_DB_PORT=5432                           # default=5432
HVS_DB_SSLMODE=verify-full                 # default=verify-full ;other options are like allow, prefer, require, verify-ca
HVS_DB_SSLCERT=/etc/hvs/hvsdbcert.pem      # default=/etc/hvs/hvsdbcert.pem

# Webservice configuration - Optional
HVS_PORT=8443
HVS_SERVER_READ_TIMEOUT=30s
HVS_SERVER_READ_HEADER_TIMEOUT=10s
HVS_SERVER_WRITE_TIMEOUT=10s
HVS_SERVER_IDLE_TIMEOUT=10s
HVS_SERVER_MAX_HEADER_BYTES=1048576

# Logging - Optional
HVS_LOG_MAX_LENGTH=300
HVS_ENABLE_CONSOLE_LOG=false

# Flavor Signing Configuration - Optional
FLAVOR_SIGNING_KEY_FILE=/etc/hvs/trusted-keys/flavor-signing.key
FLAVOR_SIGNING_CERT_FILE=/etc/hvs/certs/trustedca/flavor-signing.pem
FLAVOR_SIGNING_COMMON_NAME=HVS Flavor Signing Certificate

# SAML Configuration - Optional
SAML_KEY_FILE=/etc/hvs/trusted-keys/saml.key
SAML_CERT_FILE=/etc/hvs/certs/trustedca/saml-cert.pem
SAML_COMMON_NAME=HVS SAML Certificate

# Endorsement CA Configuration - Optional
ENDORSEMENT_CA_KEY_FILE=/etc/hvs/trusted-keys/endorsement-ca.key
ENDORSEMENT_CA_CERT_FILE=/etc/hvs/certs/trustedca/EndorsementCA.pem
ENDORSEMENT_CA_COMMON_NAME=HVS Endorsement Certificate
ENDORSEMENT_CA_ISSUER=intel-secl
ENDORSEMENT_CA_VALIDITY_YEARS=5

# Privacy CA Configuration - Optional
PRIVACY_CA_KEY_FILE=/etc/hvs/trusted-keys/privacy-ca.key
PRIVACY_CA_CERT_FILE=/etc/hvs/certs/trustedca/privacy-ca-cert.pem
PRIVACY_CA_COMMON_NAME=HVS Privacy Certificate
PRIVACY_CA_ISSUER=intel-secl
PRIVACY_CA_VALIDITY_YEARS=5

# Asset Tag Configuration - Optional
TAG_CA_KEY_FILE=/etc/hvs/trusted-keys/tag-ca.key
TAG_CA_CERT_FILE=/etc/hvs/certs/trustedca/tag-ca-cert.pem
TAG_CA_COMMON_NAME=HVS Tag Certificate
TAG_CA_ISSUER=intel-secl
TAG_CA_VALIDITY_YEARS=5

# Certificate Revocation Checks - Optional
ENABLE_EKCERT_REVOKE_CHECK=false # default=false - if true, revocation checks will be performed on EK certs
                                 # proxy settings (if applicable) must be provided - see next stion

```

## Note on Certificate Revocation Checks for TPM EK Certs

The ENABLE_EKCERT_REVOKE_CHECK setting has been added to toggle revocation checks for Endorsement Key Certs by Verification Service. The revocation check will be performed during the course of the AIK provisioning flow on the Trust Agent (`tagent setup provision-aik`).

At this stage, there are 3 possible outcomes:

1. No certs in the chain are found to be revoked: VS responds with a HTTP 200 response code
2. A certificate is found to be revoked: VS responds with a HTTP 400 response code
3. The revocation check failed due to a connection error: VS responds with a HTTP 500 response code 

If proxy settings are required to source CRL from external CAs, these can be added to the Verification Service service systemd via a drop-in config at /etc/systemd/system/hvs.service.d/proxy.conf or directly to `/opt/hvs/hvs.service` under the service section like so:

```
[Service]
Environment="https_proxy=<proxy server url>"
```

Run `systemctl daemon-reload` and restart the service after making this change.

## Configuration Options

The Verification Service configuration is stored in the
file `/etc/hvs/config.yml`:

```yaml
tls:
  cert-file: /etc/hvs/tls-cert.pem
  key-file: /etc/hvs/tls.key
  common-name: HVS TLS Certificate
  san-list: 127.0.0.1,localhost
saml:
  common:
    cert-file: /etc/hvs/certs/trustedca/saml-cert.pem
    key-file: /etc/hvs/trusted-keys/saml.key
    common-name: HVS SAML Certificate
  issuer: AttestationService
  validity-days: 1
flavor-signing:
  cert-file: /etc/hvs/certs/trustedca/flavor-signing.pem
  key-file: /etc/hvs/trusted-keys/flavor-signing.key
  common-name: HVS Flavor Signing Certificate
privacy-ca:
  cert-file: /etc/hvs/certs/trustedca/privacy-ca/privacy-ca-cert.pem
  key-file: /etc/hvs/trusted-keys/privacy-ca.key
  common-name: HVS Privacy Certificate
  issuer: intel-secl
  validity-years: 5
endorsement-ca:
  cert-file: /etc/hvs/certs/endorsement/EndorsementCA.pem
  key-file: /etc/hvs/trusted-keys/endorsement-ca.key
  common-name: HVS Endorsement Certificate
  issuer: intel-secl
  validity-years: 5
tag-ca:
  cert-file: /etc/hvs/certs/trustedca/tag-ca-cert.pem
  key-file: /etc/hvs/trusted-keys/tag-ca.key
  common-name: HVS Tag Certificate
  issuer: intel-secl
  validity-years: 5
aik-certificate-validity-years: 5
server:
  port: 8898
  read-timeout: 30s
  read-header-timeout: 10s
  write-timeout: 30s
  idle-timeout: 10s
  max-header-bytes: 1048576
log:
  max-length: 30000
  enable-stdout: true
  level: TRACE
db:
  vendor: postgres
  host: localhost
  port: "5432"
  name: hvs_db
  username: root
  password: password
  ssl-mode: allow
  ssl-cert: /etc/hvs/hvsdbsslcert.pem
  conn-retry-attempts: 5
  conn-retry-time: 1
hrrs:
  refresh-period: 2m0s
  refresh-look-ahead: 5m0s
fvs:
  number-of-verifiers: 20
  number-of-data-fetchers: 20
  skip-flavor-signature-verification: true
enable-ekcert-revoke-check: false
```


## Command-Line Options

The Verification Service supports several command-line commands that can
be executed only as the Root user:

Syntax:
```
hvs <command>
```
### Help
```
hvs help
```
Displays the list of available CLI commands.

### Start
```
hvs start
```
Starts the services.

### Stop
```
hvs stop
```
Stops the services.

### Status
```
hvs status
```
Reports whether the service is currently running.

### Uninstall
```
hvs uninstall
```
Uninstalls the service, including the deletion of all files and folders.
Database content is not removed. See section 14.1 for additional
details.

### Version
```
hvs version
```
Reports the version of the service.

### Erase-data
```
hvs erase-data
```
Deletes all non-user information from the database.  All data in the following tables will be deleted; the database schema will be preserved:

```
 flavor_host 
 flavor_flavorgroup
 flavorgroup_host
 queue
 report
 host_status
 flavorgroup
 host
 flavor
 host_credential
 tag_certificate
 audit_log_entry
 tls_policy
```

### Setup
```yaml
Usage of hvs setup:
        hvs setup <task> [--help] [--force] [-f <answer-file>]
                --help                      show help message for setup task
                --force                     existing configuration will be overwritten if this flag is set
                -f|--file <answer-file>     the answer file with required arguments

Available Tasks for setup:
        all                             Runs all setup tasks
        database                        Setup hvs database
        create-default-flavorgroup      Create default flavor groups in database
        create-dek                      Create data encryption key for HVS
        download-ca-cert                Download CMS root CA certificate
        download-cert-tls               Download CA certificate from CMS for tls
        download-cert-saml              Download CA certificate from CMS for saml
        download-cert-flavor-signing    Download CA certificate from CMS for flavor signing
        create-endorsement-ca           Generate self-signed endorsement certificate
        create-privacy-ca               Generate self-signed privacy certificate
        create-tag-ca                   Generate self-signed tag certificate
        update-service-config           Sets or Updates the Service configuration
```
## Directory Layout

The Host Verification Service installs by default to the following folders:

/etc/hvs/

This directory contains the config.yml configuration file, the database connection ssl cerificate, and the webservice TLS certificate.

/etc/hvs/

├── certs

│   ├── endorsement

│   │   ├── EndorsementCA-external.pem

│   │   └── EndorsementCA.pem

│   ├── trustedca

│   │   ├── flavor-signing.pem

│   │   ├── privacy-ca

│   │   │   └── privacy-ca-cert.pem

│   │   ├── root

│   │   │   ├── 58f6bcfcd.pem

│   │   │   ├── vmware-cert1.pem

│   │   │   └── vmware-cert2.pem

│   │   ├── saml-cert.pem

│   │   └── tag-ca-cert.pem

│   └── trustedjwt

│       └── f29aa4ab3.pem

├── config.yml

├── hvsdbsslcert.pem

├── tls-cert.pem

├── tls.key

└── trusted-keys

​    ├── endorsement-ca.key

​    ├── flavor-signing.key

​    ├── privacy-ca.key

​    ├── saml.key

​    └── tag-ca.key
