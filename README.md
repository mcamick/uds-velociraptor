# Velociraptor UDS Package
**⚠️ IMPORTANT**: Currently breaks with SSO enabled due to compatibility issues between keycloak and Velociraptor's authentication mechanism

This package provides Velociraptor - a digital forensics and incident response platform - configured for deployment using UDS (Unicorn Delivery Service).

Velociraptor is an advanced endpoint monitoring, digital forensics and cyber response platform that enables organizations to collect, analyze and monitor endpoint activities using Velociraptor Query Language (VQL) queries.

## Features

- **Digital Forensics**: Comprehensive endpoint investigation capabilities
- **Incident Response**: Real-time threat hunting and response
- **Endpoint Monitoring**: Continuous visibility into endpoint activities
- **VQL Queries**: Powerful query language for data collection and analysis
- **Multi-Platform**: Support for Windows, Linux, and macOS endpoints

## Architecture

This UDS package includes:
- **Custom Velociraptor image** built from source with security hardening
- Velociraptor server deployment with persistent storage
- Service configuration exposing GUI (8000), API (8001), and Frontend (8889) ports
- UDS Core integration with Istio gateway support
- Optional SSO integration
- Persistent volume claim for data storage

### Custom Image Features
- Based on Ubuntu 22.04 with latest Velociraptor binaries
- Runs as non-root user (UID/GID 1000) for enhanced security
- Includes all platform clients (Windows, Linux, macOS)
- Automatic certificate rotation and client repacking

## Pre-requisites

The Velociraptor Package expects to be deployed on top of [UDS Core](https://github.com/defenseunicorns/uds-core) with the following requirements:

### Dependencies
- **UDS Core**: Required for networking, ingress, and SSO capabilities
- **Persistent Storage**: Cluster must support PersistentVolumeClaims
- **Network Access**: Outbound connectivity for Velociraptor client downloads (if needed)

### Storage Requirements
- **Default**: 10Gi persistent storage for Velociraptor data
- **Configurable**: Storage class can be specified in values
- **Access Mode**: ReadWriteOnce

## Deployment

### Quick Start

1. **Build the package:**
   ```bash
   uds zarf package create
   ```

2. **Deploy with basic configuration:**
   ```bash
   uds zarf package deploy --set DOMAIN=your-domain.com --set ENABLE_SSO=false
   ```

### Configuration Options

#### Required Variables
- `DOMAIN`: The domain for accessing Velociraptor endpoints
- `ENABLE_SSO`: Enable/disable SSO integration (`true`/`false`)

#### Storage Configuration
Update `values.yaml` to specify storage class:
```yaml
persistence:
  size: 10Gi
  storageClass: "gp2"  # AWS EKS example
```

#### Security Configuration
**⚠️ IMPORTANT**: Change default credentials before production deployment!

Default admin credentials are set in `values.yaml`:
```yaml
env:
  - name: VELOX_USER
    value: "admin"
  - name: VELOX_PASSWORD
    value: "admin"  # CHANGE THIS IN PRODUCTION
```

For production, use Kubernetes secrets:
```yaml
env:
  - name: VELOX_PASSWORD
    valueFrom:
      secretKeyRef:
        name: velociraptor-credentials
        key: password
```

## Access

After deployment, Velociraptor will be available at:
- **Web Interface**: `https://velociraptor.${DOMAIN}`
- **API Endpoint**: Port 8001
- **Frontend**: Port 8889

Default login:
- **Username**: admin
- **Password**: admin (change immediately!)

## UDS Tasks (for local dev and CI)

*For local dev, this requires you install [uds-cli](https://github.com/defenseunicorns/uds-cli?tab=readme-ov-file#install)*

> [!TIP]
> To get a list of tasks to run you can use `uds run --list`!

## Troubleshooting

### Common Issues

1. **Storage Issues**: If PVC fails to provision, verify storage class availability
2. **Permission Errors**: Container runs as user 1000; ensure image compatibility
3. **Image Pull Errors**: Verify `wlambert/velociraptor` image accessibility from cluster
4. **Startup Failures**: Check pod logs for Velociraptor configuration issues

## Security Considerations

- **Credentials**: Always change default admin credentials
- **Network**: Use UDS Core network policies for segmentation
- **TLS**: Velociraptor uses self-signed certificates by default
- **RBAC**: Configure appropriate Kubernetes RBAC for the service account
- **Data**: Persistent volume contains sensitive forensics data - secure appropriately

## Support

For issues related to:
- **UDS packaging**: Create issues in this repository
- **Velociraptor application**: Refer to [Velociraptor documentation](https://docs.velociraptor.app/)
- **UDS Core**: See [UDS Core repository](https://github.com/defenseunicorns/uds-core)
