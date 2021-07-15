# Metallb deployment

## Installation
### 1. Verify requirements 
Requirements described at https://metallb.universe.tf/#requirements

### 2. Enable strictARP in kube-proxy configmap
Edit the kube-proxy configmap with the command: `kubectl edit configmap -n kube-system kube-proxy` and search for the `strictARP` attribute to make sure it is set to `true`.

```
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: "ipvs"
ipvs:
  strictARP: true
```

### 3. Install MetalLB
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/metallb.yaml
# On first install only
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```

### 4. Create a configmap file config.yml
Create a default address-pool with the folowing code:
```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.1.230-192.168.1.250
```

### 5. Apply the configmap 
`kubectl apply -f config.yml`

## References
Official deployment guide at: https://metallb.universe.tf/installation/

