# Ingress Permanent Redirect (nginx) 

## Example 

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
   annotations:
     nginx.ingress.kubernetes.io/permanent-redirect: https://nginx.redirect/destination
     nginx.ingress.kubernetes.io/permanent-redirect-code: '308'
   name: destination-home
   namespace: myNamespace
 spec:
   rules:
   - host: nginx.redirect
     http:
       paths:
       - backend:
           serviceName: http-svc
           servicePort: 80
         path: /source


curl -I  http://nginx.redirect/source
HTTP/1.1 308 
Permanent Redirect 
Location: https://nginx.redirect/destination
curl -I  http://nginx.redirect/source/bar 
HTTP/1.1 308 
Permanent Redirect 
Location: https://nginx.redirect/destination


```

## Umbauen zu google ;o) 

```
This annotation allows to return a permanent redirect instead of sending data to the upstream. For example nginx.ingress.kubernetes.io/permanent-redirect: https://www.google.com would redirect everything to Google.

```

## Refs:

  * https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/nginx-configuration/annotations.md#permanent-redirect
  * 
