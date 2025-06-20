# Velociraptor UDS Deployment Notes

## Deployment Readiness: ✅ READY

The Docker Compose configuration has been successfully converted to UDS format and is ready for cluster deployment.

## Pre-Deployment Configuration

### Required Variables
Before deployment, ensure these Zarf variables are set:
- `DOMAIN`: The domain for accessing endpoints
- `ENABLE_SSO`: Whether to enable SSO integration

### Storage Configuration
The PVC template supports configurable storage class. Update in `values.yaml`:
```yaml
persistence:
  size: 10Gi
  storageClass: "your-storage-class"  # Uncomment and set as needed
```

Common storage classes:
- AWS EKS: `gp2`
- Google GKE: `standard`
- Local development: `hostpath`

## Security Considerations

### 🔒 Production Security
**CRITICAL**: Update the admin credentials before production deployment:

Current configuration uses hardcoded credentials:
```yaml
env:
  - name: VELOX_USER
    value: "admin"
  - name: VELOX_PASSWORD
    value: "admin"  # ⚠️ CHANGE THIS
```

**Recommended**: Use Kubernetes secrets:
```yaml
env:
  - name: VELOX_PASSWORD
    valueFrom:
      secretKeyRef:
        name: velociraptor-credentials
        key: password
```

### Security Context
Applied secure defaults:
- `runAsUser: 1000`
- `runAsNonRoot: true`
- `allowPrivilegeEscalation: false`
- `fsGroup: 1000`

## Service Configuration

### Ports Exposed
- **8000**: GUI/Web Interface (primary)
- **8001**: API Interface
- **8889**: Frontend Interface

### Health Checks
- Liveness probe: HTTP check on port 8000, starts after 60s
- Readiness probe: HTTP check on port 8000, starts after 30s

## Custom Image Build

This package builds a custom Velociraptor image with the following features:
- **Security**: Runs as non-root user (velociraptor:1000)
- **Latest binaries**: Downloads latest Velociraptor release during build
- **Multi-platform**: Includes Windows, Linux, and macOS client binaries
- **Auto-configuration**: Handles certificate rotation and client repacking

### Build Process
The custom image is built from `src/velociraptor/Dockerfile` during Zarf package creation.

## Known Limitations

1. **Build Dependencies**: Requires internet access during image build for downloading Velociraptor binaries
2. **Data Persistence**: All Velociraptor data persists to `/velociraptor` in the PVC
3. **Networking**: UDS package configured for istio gateway exposure
4. **Build Time**: Initial build may take longer due to binary downloads

## Deployment Commands

1. Build the Zarf package:
   ```bash
   zarf package create
   ```

2. Deploy with required variables:
   ```bash
   zarf package deploy --set DOMAIN=your-domain.com --set ENABLE_SSO=false
   ```

## Troubleshooting

### Common Issues
- **Storage**: If PVC fails, check storage class availability
- **Permissions**: If container fails to start, may need to adjust security context
- **Image Pull**: Verify `wlambert/velociraptor` image is accessible from cluster

### Logs
Check pod logs for Velociraptor startup issues:
```bash
kubectl logs -n velociraptor deployment/velociraptor
```