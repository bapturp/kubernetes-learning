# Ingress

## Install contour ingress

```sh
kubectl apply -f https://projectcontour.io/quickstart/contour.yaml
```

## Ingress controler

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: host-ingress
spec:
  rules:
  - host: alpaca.k8s.local
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: alpaca
            port:
              number: 8080
  defaultBackend:
    service:
      name: alpaca
      port:
        number: 8080
```

## tls

```sh
kubectl create secret tls <secret-name> --cert <certificate-pem-file> --key <private-key-pem-file>
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: kubernetes.io/tls
data:
  tls.crt: <base64 encoded certificate>
  tls.key: <base64 encoded private key>
```

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: alpaca
spec:
  tls:
  - hosts:
      - alpaca.example.com
    secretName: testsecret-tls
  rules:
  - host: alpaca.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: alpaca
            port:
              number: 80
```
