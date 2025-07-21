# MCP Application Kubernetes Deployment

## Quick Start

1. **Create the namespace:**
   ```bash
   kubectl apply -f namespace.yaml
   ```

2. **Create secrets (update with your actual credentials first):**
   ```bash
   # Edit secrets.yaml to replace the base64 encoded placeholder values
   kubectl apply -f secrets.yaml
   ```

3. **Deploy the applications:**
   ```bash
   kubectl apply -f azmcpserver-deployment.yaml
   kubectl apply -f azmcpserver-service.yaml
   kubectl apply -f mcp-client-deployment.yaml
   kubectl apply -f mcp-client-service.yaml
   kubectl apply -f mcp-frontend-deployment.yaml
   kubectl apply -f mcp-frontend-service.yaml
   ```

4. **Check deployment status:**
   ```bash
   kubectl get pods -n mcp-app
   kubectl get services -n mcp-app
   ```

## Files Overview

- `namespace.yaml`: Creates the mcp-app namespace
- `secrets.yaml`: Contains sensitive data (passwords, API keys)
- `azmcpserver-deployment.yaml`: Azure MCP Server deployment
- `azmcpserver-service.yaml`: Azure MCP Server service
- `mcp-client-deployment.yaml`: MCP Client deployment
- `mcp-client-service.yaml`: MCP Client service
- `mcp-frontend-deployment.yaml`: MCP Frontend deployment
- `mcp-frontend-service.yaml`: MCP Frontend service

## Services

- **azmcpserver-service**: Internal service on port 8005
- **mcp-client-service**: Internal service on port 8080
- **mcp-frontend-service**: External LoadBalancer service on port 80

## Important Notes

1. **Update Secrets**: Before deploying, encode your actual credentials in base64:
   ```bash
   echo -n "your-actual-password" | base64
   echo -n "your-actual-api-key" | base64
   ```

2. **Network Communication**: Services communicate using internal DNS names:
   - `azmcpserver-service.mcp-app.svc.cluster.local:8005`
   - `mcp-client-service.mcp-app.svc.cluster.local:8080`

3. **Image Pull Policy**: Set `imagePullPolicy: Always` if using latest tags or private registry

4. **Resource Limits**: Adjust CPU/memory limits based on your cluster capacity

## Port Mapping

| Docker | Kubernetes Service | Access |
|--------|-------------------|---------|
| 8005 | azmcpserver-service:8005 | Internal |
| 8080 | mcp-client-service:8080 | Internal |
| 80 | mcp-frontend-service:80 | External (LoadBalancer) |
