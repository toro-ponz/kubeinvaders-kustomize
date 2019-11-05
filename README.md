# Kubeinvaders Kustomize

This repository support installation kubernetes resource of [Kubeinvaders](https://github.com/lucky-sideburn/KubeInvaders) by Kustomize.

![image](https://user-images.githubusercontent.com/20720712/68214986-c65b1480-0021-11ea-83a3-b449e0be8520.png)

## Usage

### 0. Install NGINX Ingress Controller

https://github.com/kubernetes/ingress-nginx

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/cloud-generic.yaml
```

### 1. Apply by kustomize

```shell
kubectl kustomize ./overlay/local | kubectl apply -f -
```

### 2. Get secret token

```shell
kubectl describe secret $(kubectl get secret -n kubeinvaders | grep 'kubeinvaders-service-account' | awk '{ print $1 }') -n kubeinvaders | grep 'token:' | awk '{ print $2 }'
```

or

```shell
kubectl get secret -n kubeinvaders
kubectl describe secret kubeinvaders-service-account-token-xxxxx -n kubeinvaders
```

### 3. Edit ConfigMap

```shell
kubectl edit configmap variables -n kubeinvaders
```

```diff
data:
  alienproximity: "15"
  hitslimit: "0"
  routeHost: local-kubeinvaders.com
  targetNamespaces: kube-system
- token: ""
+ token: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" // paste secret token
  updatetime: "0.5"
```

### 4. Restart Pod

```shell
kubectl rollout restart deployment -n kubeinvaders
```

or

```shell
kubectl delete pods --all -n kubeinvaders
```

### 5. Edit /etc/hosts

```shell
sudo vi /etc/hosts
```

```diff
+ # kubernetes ingress hosts
+ 127.0.0.1 local.kubeinvaders.com
```

### 6. Access to Pod

https://local.kubeinvaders.com

### 7. Have fun

| Input | Action |
---|---
| space | Hit a bullet. |
| n | Change namespace. |
| a | Switch to automatic mode. |
| m | Switch to manual mode. |
| h | Show special keys. |
| q | Hide help for special keys. |
| i | Show pod's name. Move the ship towards an alien. |

quoted from [Kubeinvaders](https://github.com/lucky-sideburn/KubeInvaders).
