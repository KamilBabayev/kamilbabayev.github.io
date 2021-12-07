---
layout: post
title:  "Kubernetes RBAC"
categories: kubernetes
tags: kubernetes
author: Kamil Babayev
mathjax: true
---

* content
{:toc}

blog_info




{% raw %}
## Heading

## Other links

## PDF Version Preview

# kubernetes

#### Create user with certificates and provide it access to pods.

First let create key and csr file for this user for accessing cluster

create openssl key
```
openssl req -new -key kamil.key -out kamil.csr
```

create openssl csr file from above key
```
openssl req -new -key kamil.key -out kamil.csr
Common Name (e.g. server FQDN or YOUR name) []:kamilb
```
Note: set common name field of csr same as username

get Create CertificateSigningRequest config from kubernetes doc

create kamilb.yaml  file with below config
```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: kamilb
spec:
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ216Q0NBWU1DQVFBd1ZqRUxNQWtHQTFVRUJoTUNRVlV4RXpBUkJnTlZCQWdNQ2xOdmJXVXRVM1JoZEdVeApJVEFmQmdOVkJBb01HRWx1ZEdWeWJtVjBJRmRwWkdkcGRITWdVSFI1SUV4MFpERVBNQTBHQTFVRUF3d0dhMkZ0CmFXeGlNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXJkNDZLUVNzNjlra0JlM28KTGQ3bndXa25OclRuTzB3Z1VDaGpZWldSa1Y3cDUwZmVRdy9NRE1mb0V0bkNHS0RCUEZiSU8rcnBoRGk5dGF3ZApZQ043N042dzc5dW5rVjd5L2xCUWtiTTQ0RXcwY0g3SjQ0K1RYNnRMbERnRXlyME50U3FXTW12WUlKay9CVUQ4CnB3RjR1OEdmK2JFVzJRUzlEdktGV2xndWJDREZsU1U2dXRvOFlQMk41VU91dnVXeitnQi9temQyeWwwKzNhc3gKMkJIWTh2STQ5NGtNKzd3RXRoaXlWZjQ3QWVZYk1LMTdzMW1MVUJYa000cFlOZUJmcmgxNnB5YVNsSXJLM0tnbgpuQW1LeDVyUWRxZWlWKzJlUzF1Wi96OENFWXBxQ01YRnY1YzkvY2UxSENFaG9ydXpvOXZBZDdRK0ZpdDJZWXVHCkkyVlpIUUlEQVFBQm9BQXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBQWp6dGR2ZVBRNzdTT29MT1FjQWhRNVIKbUFKUlBQMU5KSzZkQ0g3K1pkcFpNNGVlcTRXMnUvMWJSOVcvZHVKRmRRUm16RjZkeVRGaisyZHBOaDN3TFdNQQpUdExtQ3NRVE9ueXRTem9Cck5WMU5SeXFnNHhmWnI5WXltMy9MZjRkcmIvclB3dWE0RnlTYnhpdktWQ2F0QlNTCkZWZ3F4TThVTVJYVjZjd2tPZ3EzUllsNUpTUXhwL1lXZyttRFVjZ0NoLzIvT3pXaDZRQUlLZVN3U0FEcE5yUVYKMzhUeXFGeE9uYlZFTmtVbDI3SnpqcUFERWpjVW5tRnBGdWZGOVMyKys0Y3QzbGExL2ZnQkJidGJzQnl4dDdadwpobjRGTEIyeWtxUTNQVVZDS1pSYTN5RWxUR2orL2t0dzdFSDdISS8vU1JyMXNVK1B3amZrbTRnS3JOOEZnQjA9Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # set as you need or comment
  usages:
  - client auth
```

encode kamilb.csr file content with base64 and paste it inside CSR object file in request field.
we will submit this file to kube api and will approve it to get singed cert from kube api to use it  later to access kube api.
```
cat kamilb.csr | base64 -w 0
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ216Q0NBWU1DQVFBd1ZqRUxNQWtHQTFVRUJoTUNRVlV4RXpBUkJnTlZCQWdNQ2xOdmJXVXRVM1JoZEdVeApJVEFmQmdOVkJBb01HRWx1ZEdWeWJtVjBJRmRwWkdkcGRITWdVSFI1SUV4MFpERVBNQTBHQTFVRUF3d0dhMkZ0CmFXeGlNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXJkNDZLUVNzNjlra0JlM28KTGQ3bndXa25OclRuTzB3Z1VDaGpZWldSa1Y3cDUwZmVRdy9NRE1mb0V0bkNHS0RCUEZiSU8rcnBoRGk5dGF3ZApZQ043N042dzc5dW5rVjd5L2xCUWtiTTQ0RXcwY0g3SjQ0K1RYNnRMbERnRXlyME50U3FXTW12WUlKay9CVUQ4CnB3RjR1OEdmK2JFVzJRUzlEdktGV2xndWJDREZsU1U2dXRvOFlQMk41VU91dnVXeitnQi9temQyeWwwKzNhc3gKMkJIWTh2STQ5NGtNKzd3RXRoaXlWZjQ3QWVZYk1LMTdzMW1MVUJYa000cFlOZUJmcmgxNnB5YVNsSXJLM0tnbgpuQW1LeDVyUWRxZWlWKzJlUzF1Wi96OENFWXBxQ01YRnY1YzkvY2UxSENFaG9ydXpvOXZBZDdRK0ZpdDJZWXVHCkkyVlpIUUlEQVFBQm9BQXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBQWp6dGR2ZVBRNzdTT29MT1FjQWhRNVIKbUFKUlBQMU5KSzZkQ0g3K1pkcFpNNGVlcTRXMnUvMWJSOVcvZHVKRmRRUm16RjZkeVRGaisyZHBOaDN3TFdNQQpUdExtQ3NRVE9ueXRTem9Cck5WMU5SeXFnNHhmWnI5WXltMy9MZjRkcmIvclB3dWE0RnlTYnhpdktWQ2F0QlNTCkZWZ3F4TThVTVJYVjZjd2tPZ3EzUllsNUpTUXhwL1lXZyttRFVjZ0NoLzIvT3pXaDZRQUlLZVN3U0FEcE5yUVYKMzhUeXFGeE9uYlZFTmtVbDI3SnpqcUFERWpjVW5tRnBGdWZGOVMyKys0Y3QzbGExL2ZnQkJidGJzQnl4dDdadwpobjRGTEIyeWtxUTNQVVZDS1pSYTN5RWxUR2orL2t0dzdFSDdISS8vU1JyMXNVK1B3amZrbTRnS3JOOEZnQjA9Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
```

i have pasted this output to request: filed in above CSR yaml file

Now create csr object using this yaml file
```
kubectl create -f kamilb.yaml
```

if we get csr file we will see it is created and in Pending state
```
master# kubectl get certificatesigningrequests.certificates.k8s.io 
NAME     AGE   SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
famil    44m   kubernetes.io/kube-apiserver-client   kubernetes-admin   24h                 Approved,Issued
kamilb   50s   kubernetes.io/kube-apiserver-client   kubernetes-admin   24h                 Pending
```

now approve csr request as it is request by us.
```
master# kubectl certificate approve kamilb
certificatesigningrequest.certificates.k8s.io/kamilb approved
```
Now we will get certificate from approved  csr object to decode it and get cert
```
kubectl get certificatesigningrequests.certificates.k8s.io kamilb -o yaml
```

copy certificate field from output of above command and decode with bas64
```
echo LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM5akNDQWQ2Z0F3SUJBZ0lRV25uMXAwbWdzOTAvU0FDUjFvUUR0akFOQmdrcWhraUc5dzBCQVFzRkFEQVYKTVJNd0VRWURWUVFERXdwcmRXSmxjbTVsZEdWek1CNFhEVEl4TVRFeE1EQTRNalF4TWxvWERUSXhNVEV4TVRBNApNalF4TWxvd0VURVBNQTBHQTFVRUF4TUdZVzVuWld4aE1JSUJJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBCk1JSUJDZ0tDQVFFQTByczhJTHRHdTYxakx2dHhWTTJSVlRWMDNHWlJTWWw0dWluVWo4RElaWjBOdHYxRm1FUVIKd3VoaUZsOFEzcWl0Qm0wMUFSMkNJVXBGd2ZzSjZ4MXF3ckJzVkhZbGlBNVhwRVpZM3ExcGswSDQzdndoYmUrWgo2MVNrVHF5SVBYUUwrTWM5T1Nsbm0xb0R2N0NtSkZNMUlMRVI3QTVGZnZKOEdFRjJ6dHBoaUlFM25vV210c1lvCnJuT2wzc2lHQ2ZGZzR4Zmd4eW8ybmlneFNVekl1bXNnVm9PM2ttT0x1RVF6cXpkakJ3TFJXbWlESWYxcExaejIKalVnald4UkhCM1gyWnVVV1d1T09PZnpXM01LaE8ybHEvZi9DdS8wYk83c0x0MCt3U2ZMSU91TFdxb3RuVm1GbApMMytqTy82WDNDKzBERHk5aUtwbXJjVDBnWGZLemE1dHJRSURBUUFCbzBZd1JEQVRCZ05WSFNVRUREQUtCZ2dyCkJnRUZCUWNEQWpBTUJnTlZIUk1CQWY4RUFqQUFNQjhHQTFVZEl3UVlNQmFBRkNTb0VQUmVhbGtnS2s5NlU3MUYKYnBxQkE3eFRNQTBHQ1NxR1NJYjNEUUVCQ3dVQUE0SUJBUUFteDRHUk9BK3VYWlh3Nmo4MkdqOUFyaXcvcGtiNApudWlSeitRMlgyVWlHUEUxMHZWZ0NFSk16cHF5VFl2T2lBUWQ1cUsyZ2wzclB4T0NxTzNubGI5L3JEcTFpU2FwCkNGdmFPYzczSURZeFRpVFVxWmtzeVBqRDNTT2N4RWN6QVA1RHQzVE0rdXI2NnQ0MnZBMG53bGZ6N1VxUGpjQi8KMmp6OHlNNWNVQVpMcWhycFM3NGxIQUYzTG82SnJkN3ZaZWFrM0ZkWHlvWHdCNnpnemV5NEJiQWNRRlBDazFUNwpzQTZvSjl0UlMrMk10YU5QMmlyQW1KYmJ6LzJkZ0FJaGJPUmZRZy9jTmZjV01kcUk5RTR3bUNHTEJaYmwxSExUCjU5UklodkhuT0Rlc3BXTGdDTWFZa2g5NS94TkNPWkpEQ051VTg1TWpNandIN1V4ZDUrWkFUamhZCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K | base64 --decode
-----BEGIN CERTIFICATE-----
MIIC9jCCAd6gAwIBAgIQWnn1p0mgs90/SACR1oQDtjANBgkqhkiG9w0BAQsFADAV
MRMwEQYDVQQDEwprdWJlcm5ldGVzMB4XDTIxMTExMDA4MjQxMloXDTIxMTExMTA4
MjQxMlowETEPMA0GA1UEAxMGYW5nZWxhMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
MIIBCgKCAQEA0rs8ILtGu61jLvtxVM2RVTV03GZRSYl4uinUj8DIZZ0Ntv1FmEQR
wuhiFl8Q3qitBm01AR2CIUpFwfsJ6x1qwrBsVHYliA5XpEZY3q1pk0H43vwhbe+Z
61SkTqyIPXQL+Mc9OSlnm1oDv7CmJFM1ILER7A5FfvJ8GEF2ztphiIE3noWmtsYo
rnOl3siGCfFg4xfgxyo2nigxSUzIumsgVoO3kmOLuEQzqzdjBwLRWmiDIf1pLZz2
jUgjWxRHB3X2ZuUWWuOOOfzW3MKhO2lq/f/Cu/0bO7sLt0+wSfLIOuLWqotnVmFl
L3+jO/6X3C+0DDy9iKpmrcT0gXfKza5trQIDAQABo0YwRDATBgNVHSUEDDAKBggr
BgEFBQcDAjAMBgNVHRMBAf8EAjAAMB8GA1UdIwQYMBaAFCSoEPRealkgKk96U71F
bpqBA7xTMA0GCSqGSIb3DQEBCwUAA4IBAQAmx4GROA+uXZXw6j82Gj9Ariw/pkb4
nuiRz+Q2X2UiGPE10vVgCEJMzpqyTYvOiAQd5qK2gl3rPxOCqO3nlb9/rDq1iSap
CFvaOc73IDYxTiTUqZksyPjD3SOcxEczAP5Dt3TM+ur66t42vA0nwlfz7UqPjcB/
2jz8yM5cUAZLqhrpS74lHAF3Lo6Jrd7vZeak3FdXyoXwB6zgzey4BbAcQFPCk1T7
sA6oJ9tRS+2MtaNP2irAmJbbz/2dgAIhbORfQg/cNfcWMdqI9E4wmCGLBZbl1HLT
59RIhvHnODespWLgCMaYkh95/xNCOZJDCNuU85MjMjwH7Uxd5+ZATjhY
-----END CERTIFICATE-----
```

create kamilb.crt file and paste decoded certiicate in it.


Now add user entry to kubeconfig file as below with kamilb's new key and cert
```
kubectl config set-credentials kamilb --client-key=/root/key/kamilb.key --client-certificate=/root/key/kamilb.crt
```

get users and contexts
```
kubectl config get-clusters
kubectl config get-users
```

create new context
```
kubectl config set-context kamilb --cluster=kubernetes --user=kamilb
kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
          famil                         kubernetes   famil              
          kamil@kubernetes              kubernetes   kamil              
          kamilb                        kubernetes   kamilb             
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin  
```

switch to new context kamilb
```
kubectl config use-context kamilb
kubectl get pods
Error from server (Forbidden): pods is forbidden: User "kamilb" cannot list resource "pods" in API group "" in the namespace "default"
```

we have created user and context, switched to that context. bu out user dont have privileges to access api.
we will create role and rolebinding for this to manage pods with our new user.

```
kubectl create role kamilb-pod-access --verb=* --resource=pod
kubectl create rolebinding kamilb-role-binding --role=kamilb-pod-access --user=kamilb
```

```
master# kubectl describe role kamilb-pod-access 
Name:         kamilb-pod-access
Labels:       <none>
Annotations:  <none>
PolicyRule:
  Resources  Non-Resource URLs  Resource Names  Verbs
  ---------  -----------------  --------------  -----
  pods       []                 []              [*]

master# kubectl describe rolebindings.rbac.authorization.k8s.io kamilb-role-binding 
Name:         kamilb-role-binding
Labels:       <none>
Annotations:  <none>
Role:
  Kind:  Role
  Name:  kamilb-pod-access
Subjects:
  Kind  Name    Namespace
  ----  ----    ---------
  User  kamilb  

```

Now let us switch to our context and check pod management with kamilb user
```
master# kubectl config use-context kamilb
Switched to context "kamilb".

master# kubectl get pods
NAME    READY   STATUS    RESTARTS       AGE
demo    1/1     Running   4 (16s ago)    17h
demo2   1/1     Running   2 (167m ago)   17h

master# kubectl run web --image=nginx
pod/web created

master# kubectl delete pod --force --grace-period=0 web 
warning: Immediate deletion does not wait for confirmation that the running resource has been terminated. The resource may continue to run on the cluster indefinitely.
pod "web" force deleted
 
```

```
kubectl get deployments.apps 
Error from server (Forbidden): deployments.apps is forbidden: User "kamilb" cannot list resource "deployments" in API group "apps" in the namespace "default"
```
we can not access deplyments because role grants only pod management level access.


we use kubectl auth command to check our and other users accesses
```
kubectl auth can-i get pods
kubectl auth can-i get depl
kubectl auth can-i get deployments
kubectl auth can-i get deployments --as kamilb
kubectl auth can-i get pods --as kamilb
```

## Sayonara

Meet u again!

{% endraw %}
