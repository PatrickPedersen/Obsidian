### Install Metallb:
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.9/config/manifests/metallb-native.yaml
```

#### Apply Metallb config:
```bash
---
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: k3s-lb-pool
  namespace: metallb-system
spec:
  addresses:
  - 192.168.1.240-192.168.1.250
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: k3s-lb-pool
  namespace: metallb-system
spec:
  ipAddressPools:
  - k3s-lb-pool
```
### Install Traefik:
```bash
helm repo add traefik https://traefik.github.io/charts
helm repo update
```

traefik-values.yaml
```bash
additionalArguments:
  - '--serversTransport.insecureSkipVerify=true' 
service:
  spec:
    externalTrafficPolicy: Local
```

```bash
helm upgrade --install traefik traefik/traefik --create-namespace --namespace traefik --values ./traefik-values.yaml
```

traefik-dashboard.yaml
```bash
apiVersion: traefik.io/v1alpha1
kind: IngressRoute
metadata:
  name: dashboard
  namespace: traefik
spec:
  entryPoints:
    - web
    - websecure
  routes:
    - match: Host('traefik.gruppe1.lan') && (PathPrefix('/dashboard') || PathPrefix('/api'))
      kind: Rule
      services:
        - name: api@internal
          kind: TraefikService
```