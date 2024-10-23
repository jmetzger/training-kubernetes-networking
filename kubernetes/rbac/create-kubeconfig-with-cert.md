# Create user for kubeconfig with cert 

## Step 0: create an new rolebinding for the group (we want to use) 

```
kubectl create rolebinding developers --clusterrole=view --group=developers
```

## Step 1: on your client: create private certificate

```
cd
mkdir -p certs
# create your private key 
openssl genrsa -out ~/certs/jochen.key 4096
```

## Step 2: on your client: create csr (certificate signing request)

```
nano ~/certs/jochen.csr.cnf
```

```
[ req ]
default_bits = 2048
prompt = no
default_md = sha256
distinguished_name = dn
[ dn ]
CN = jochen
O = developers
[ v3_ext ]
authorityKeyIdentifier=keyid,issuer:always
basicConstraints=CA:FALSE
keyUsage=keyEncipherment,dataEncipherment
extendedKeyUsage=serverAuth,clientAuth
```

```
# Create Certificate Signing Request
openssl req -config ~/certs/jochen.csr.cnf -new -key ~/certs/jochen.key -nodes -out ~/certs/jochen.csr
openssl req -in certs/jochen.csr --noout -text
```


## Step 3: Send approval request to server 

```
# get csr (base64 decoded)
cat ~/certs/jochen.csr | base64 | tr -d '\n'
```

```
cd certs
nano jochen-csr.yaml
```

```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: jochen-authentication
spec:
  signerName: kubernetes.io/kube-apiserver-client
  groups:
    - system:authenticated
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJRWF6Q0NBbE1DQVFBd0pqRVBNQTBHQTFVRUF3d0dhbTlqYUdWdU1STXdFUVlEVlFRS0RBcGtaWFpsYkc5dwpaWEp6TUlJQ0lqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FnOEFNSUlDQ2dLQ0FnRUEwOTlSMkpxMmowUVk4TW4wClRsMCtyMGEyL0JJNVNqU3BXVHlvS045cGlndURSaVZLZzM1NkxaT3NOWkxad2FOODcwak9ibTkramVzNUF4N3kKa2NpUU5XOU5LanA0Yys5a1VNUDZVOFFtZENRRk9GMGdPaXI1Q25FUUo0Z1RtYlFkMkExUkcyN2VnQ1crWjVYcgpXM2pVMDFiMHRhNGFwcEsySFd2MkRaY0JZZE1HSjFSeSs3SFMzWXFFMXJ1amxMcDNqZU93SThHSGtmOU1ZMjZkCnhQYmZBbUJKWUxxWE1ZdEdiMXE1bFlTd0oxdk1BWk8rQTd2T1ZmQ2tRMmtWbE02bGRnbkszaXVBbmRqMGEzbUYKSlJuQ2F2V3o5SHlsWDJMU2IzVGlvdXI5U2VqVklwd21ZcGVPK2FrYkIzN1BaYmdVOUlRcm9nK1dOVkFlQ3BYVwpnc3pzU1R3SmpGRWdlZzQ0YUU3MVViMDlReWcwdE9TNm5nRXJQOXRWR1V3eml5aEFsdDVIdVNxazI2aDlvUjBDCllKL0J5Q29UMHJnODAvUXpWc3ZFNklzZDZjNTFPVThuc0g4K2NFd1hYQndKcGlYMUhLTWt4WHd2NTRTdTA0KzMKcUcyaHpRMWlSZUNKRzJKbmt3Y0t6OGVsMkxVRU4rS2NBRU9YUGg1RjFwRUFhQmphUkcybU5vcC9taTJpM0FKUgorT0NTbkN0RmhueDVZaDJUTGZDUDhOYUhzTkRrZEE5RlBUWWUwN1pKZmZrQ2pva2RmOGM1SVpDSHhBdkx2Yk1OCk9HZVgzcm9aWW9NbkV3VHI4ME9rVGtFbHpOZnN6OFpWa1RKVURYK1AyVnlUNXEyVHNKSldNN1lnVnkvQTk0NmYKSzdld3BldGR5R3JDUnljWDVJc3VPaVNJYllVQ0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQkN3VUFBNElDQVFDWgpEYk9yS2RzdVdmY01iKzdHaTNJb240Y3VQN2k1Q2VQa3BQeWpkdkdHclgvUGRWWVRTaThCbTZ6OU5ZbUF5UW9kClUrcEROeDlRNURJMjFCc3Y2UVNtWjlGUE1vZXU4V0NDcEphZFZRWm1WRWV1WlNmSjFrLy9aZEozK2Vib1ZwcUIKbG9ETEU1NTZQcnpEaGJTalB6aDl5U2oxU0k0QUZVaDB3VWwrK0tVZy94OXZSRzVZeFlwMVhCNUEyaUFQTTlxSQpRdnVaM2VTbUc4cVlDTkZYblR3UmpHZkdPeGtZczNDZG1NdjU2Q3hIemF6SjNNWEd2czNIYXcvbzdUcWdDVmQrCjRaRXNkOU4wcm4vU2c4ZUZ4ZGlvTER2RG03TEJLSlRXK2FyZjBOVDVyOHUvMTd3elhBeE9zQnpidk5VUXJPY3IKRUd1MFAyNjFpY0Yzb1VNZUdFeHBXTnZLTmlMOEk4eVl1NE5MRW15cFcwRVRVdnVUbVVIRFBUSXdZWkxBTmJaeApaS3RQS0VPL3RpZjVLQUVkVTdkbzV5bDhlWVlqTmk1ZGErdVJncjYxRkpOTkgxRzdsOWFvQlRhcUJHSWlPd1hSCmpvZzBFbW1Dczc4dnNBMHVDTXVLOUlWRmM4dzRTVkJ2NFVWL3U0NkZSL3JhL2dPUGxEVEd0MzBsK0lSOXYyZEUKRzcvOUV1UXQwTFhGeVpldnlCdFhDMEZ4NWtaQ2RKZ2h2R1RxVHVpVjFNeDVDTkNZUmoxaDhJam5RcVZvdU5XbApUcFl4QWNNclF3MU44Y1BMcnVxekQrTWg2RHU3emUzUUpRQUUvWmdFeUNDSzJXL2g5WFh1ZldSWHRFYW9pbUZHCldOQVJ0eUxCRG5lTzRWZHM0SHpkRjFPRzM4Q2FValdtOXp4aFdIRWp1Zz09Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
  usages:
  - client auth
```

```
kubectl apply -f jochen-csr.yaml
kubectl get -f jochen-csr.yaml
# show me the current state -> pending
kubectl describe -f jochen-csr.yaml
```

## Step 4: approve signing request

```
kubectl certificate approve jochen-authentication
# or:
kubectl certificate approve -f jochen-csr.yaml

# see, that it is approved
kubectl describe -f jochen-csr.yaml 
```

## Step 5: get the approved certificate to be used

```
kubectl get csr jochen-authentication -o jsonpath='{.status.certificate}' | base64 --decode > ~/certs/jochen.crt
```

## Step 6: construct kubeconfig for new user

```
cd
cd certs
```

```
# create new user 
kubectl config set-credentials jochen --client-certificate=jochen.crt --client-key=jochen.key
```

```
# add a new context
kubectl config set-context jochen --user=jochen --cluster=kubernetes 
```

## Step 7: Use and test the new context 

```
kubectl config use-context jochen
kubectl get pods
```

## Ref:

  * https://kb.leaseweb.com/kb/users-roles-and-permissions-on-kubernetes-rbac/kubernetes-users-roles-and-permissions-on-kubernetes-rbac-create-a-certificate-based-kubeconfig/
