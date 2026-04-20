# 🏁 picoCTF Writeup: KSECRETS

## 📌 Challenge Overview

Author: Darkraicg492

Description
We have a kubernetes cluster setup and flag is in the secrets. You think you can get it?
Additional details will be available after launching your challenge instance.

## Walkthrough
```python
cat kubeconfig.yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJlRENDQVIyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWpNU0V3SHdZRFZRUUREQmhyTTNNdGMyVnkKZG1WeUxXTmhRREUzTnpZMk9EQXlORFF3SGhjTk1qWXdOREl3TVRBeE56STBXaGNOTXpZd05ERTNNVEF4TnpJMApXakFqTVNFd0h3WURWUVFEREJock0zTXRjMlZ5ZG1WeUxXTmhRREUzTnpZMk9EQXlORFF3V1RBVEJnY3Foa2pPClBRSUJCZ2dxaGtqT1BRTUJCd05DQUFRalJFZ0FnN2Qyd254OU9udDBCZG5LQ25Zd3VjMzdHZDlPdEgzbnlRYVcKaWdkNU1vTklCN29iUC9qU0pFdmJKYTJmS1V1Vmp2N0JaRnExMUY5UGEvWlRvMEl3UURBT0JnTlZIUThCQWY4RQpCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBZEJnTlZIUTRFRmdRVVk4VzlWbHZ6dGNBZ0x5LzdpV3NLCmc5VUVzR0F3Q2dZSUtvWkl6ajBFQXdJRFNRQXdSZ0loQVBpUkE5T3E3MVo0T0J5K3ZQLzZTS2RQVGkyZ3dweXkKa0EvOGxJYnZudjBmQWlFQWpXZy9zRkx5WVdzeWNPc0JYT2V5NkQ0T1RzVWV1OGdGd0Uyd2h2RTFIb1E9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://127.0.0.1:6443
  name: default
contexts:
- context:
    cluster: default
    user: default
  name: default
current-context: default
kind: Config
users:
- name: default
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJrRENDQVRlZ0F3SUJBZ0lJY2hnK1QrYWVYeE13Q2dZSUtvWkl6ajBFQXdJd0l6RWhNQjhHQTFVRUF3d1kKYXpOekxXTnNhV1Z1ZEMxallVQXhOemMyTmpnd01qUTBNQjRYRFRJMk1EUXlNREV3TVRjeU5Gb1hEVEkzTURReQpNREV3TVRjeU5Gb3dNREVYTUJVR0ExVUVDaE1PYzNsemRHVnRPbTFoYzNSbGNuTXhGVEFUQmdOVkJBTVRESE41CmMzUmxiVHBoWkcxcGJqQlpNQk1HQnlxR1NNNDlBZ0VHQ0NxR1NNNDlBd0VIQTBJQUJBWGdFUWVFb2cxcFRveTYKd2tza0todDgwTzg4NVlvdVZxd1d4WTZNbmRPaS92VTR1dHdRY1o5MXV0bEhmeXZaMjlxNFZmaGRYVm13WUlodQpkVGtFTjhlalNEQkdNQTRHQTFVZER3RUIvd1FFQXdJRm9EQVRCZ05WSFNVRUREQUtCZ2dyQmdFRkJRY0RBakFmCkJnTlZIU01FR0RBV2dCUkY2OXNKQU0yeU1SUHRYWU8vVW5YVVJNWXFYekFLQmdncWhrak9QUVFEQWdOSEFEQkUKQWlCOVZPWmRlVnBRSUhrOFJzZndJaWhxOEVyV0dZL2hNS3NsT2l2TzhRZXlGZ0lnS1UxaU5lODUzM3NoZmgxdgptYjB6ZFN6MFZuNlhHbzZvSUxteVRHUmJLT2s9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0KLS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJkekNDQVIyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWpNU0V3SHdZRFZRUUREQmhyTTNNdFkyeHAKWlc1MExXTmhRREUzTnpZMk9EQXlORFF3SGhjTk1qWXdOREl3TVRBeE56STBXaGNOTXpZd05ERTNNVEF4TnpJMApXakFqTVNFd0h3WURWUVFEREJock0zTXRZMnhwWlc1MExXTmhRREUzTnpZMk9EQXlORFF3V1RBVEJnY3Foa2pPClBRSUJCZ2dxaGtqT1BRTUJCd05DQUFRL3RlK3A2S3VPRnptOUNuSkR3WG9XUVcrTCtlVVZFTVdrRmxYZGdGMTMKM1EyeE5uQXJtZWw2bGZOQjZzWVAveVcvU0FoRzRFUUR3eWRXV1RyYTBPOGlvMEl3UURBT0JnTlZIUThCQWY4RQpCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBZEJnTlZIUTRFRmdRVVJldmJDUUROc2pFVDdWMkR2MUoxCjFFVEdLbDh3Q2dZSUtvWkl6ajBFQXdJRFNBQXdSUUloQUk3dGpVL0ZSc3VpUUJIMHlCbG9VVzNJcVV5RWppYmQKcEg5dHhER3VZWlVzQWlCNjhtQ2taSFB1dDJJcENWL29YNXZlVE1zekYxOGFZUzFYc011TlJIOFpjQT09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    client-key-data: LS0tLS1CRUdJTiBFQyBQUklWQVRFIEtFWS0tLS0tCk1IY0NBUUVFSU1UWGk1QkxIYXhkNXZJci9zM2JhdGpPbm9PVUM4eWtFQnZGaXRNcXJNQ2tvQW9HQ0NxR1NNNDkKQXdFSG9VUURRZ0FFQmVBUkI0U2lEV2xPakxyQ1N5UXFHM3pRN3p6bGlpNVdyQmJGam95ZDA2TCs5VGk2M0JCeApuM1c2MlVkL0s5bmIycmhWK0YxZFdiQmdpRzUxT1FRM3h3PT0KLS0tLS1FTkQgRUMgUFJJVkFURSBLRVktLS0tLQo=
```

```python

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ kubectl --kubeconfig=kubeconfig.yaml --insecure-skip-tls-verify=true get namespaces
NAME              STATUS   AGE
default           Active   5m54s
kube-node-lease   Active   5m54s
kube-public       Active   5m54s
kube-system       Active   5m54s
picoctf           Active   5m45s
```

```python
kubectl --kubeconfig=kubeconfig.yaml --insecure-skip-tls-verify=true get secrets -n picoctf
NAME         TYPE     DATA   AGE
ctf-secret   Opaque   1      6m26s
```

```python
kubectl --kubeconfig=kubeconfig.yaml --insecure-skip-tls-verify=true --namespace=picoctf get secret ctf-secret -o yaml
apiVersion: v1
data:
  flag: cGljb0NURntrczNjcjM3NV80MW43X3M0ZjNfNTJmNjAzYzR9Cg==
kind: Secret
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","data":{"flag":"cGljb0NURntrczNjcjM3NV80MW43X3M0ZjNfNTJmNjAzYzR9Cg=="},"kind":"Secret","metadata":{"annotations":{},"name":"ctf-secret","namespace":"picoctf"},"type":"Opaque"}
  creationTimestamp: "2026-04-20T10:17:36Z"
  name: ctf-secret
  namespace: picoctf
  resourceVersion: "386"
  uid: 14b95e4b-954e-4ead-8ddb-1f884b710056
type: Opaque
```

## 🏁 Final Flag

```bash
picoCTF{ks3cr375_41n7_s4f3_52f603c4}
```
