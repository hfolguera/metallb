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
Obtain the last metallb resources:
```
wget https://raw.githubusercontent.com/metallb/metallb/v0.14.3/config/manifests/metallb-native.yaml
```

Execute the following command:
```
kubectl apply -f metallb-native.yaml
kubectl apply -f l2-advertisement.yaml
```

### 4. Apply the addresses configuration
`kubectl apply -f address-pools.yaml`

## References
Official deployment guide at: https://metallb.universe.tf/installation/

