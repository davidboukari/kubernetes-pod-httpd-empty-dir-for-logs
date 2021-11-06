# kubernetes-pod-httpd-empty-dir-for-logs


## Create a pod
* emptyDir for logs /usr/local/apache2/logs
 
```
tee apache-http<<EOF
apiVersion: v1
kind: Pod
metadata:
  name: apache-http
  labels:
    app: apache
    env: dev
spec:
  containers:
  - name:  apache-http
    image: httpd:latest
    securityContext:
      privileged: true
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: logs
      mountPath: /usr/local/apache2/logs
    ports:
    - containerPort: 80
    - containerPort: 443
    env:
    - name: APP_COLOR
  volumes:
  - name: logs
    emptyDir: {}
EOF
```

## port-forward on the pod
```
kubectl -n dev port-forward apache-http 9999:80 
Forwarding from 127.0.0.1:9999 -> 80
Forwarding from [::1]:9999 -> 80
Handling connection for 9999
Handling connection for 9999
```

## Test the url
```
kubectl -n dev port-forward apache-http 9999:80 
curl http://localhost:9999/
<html><body><h1>It works!</h1></body></html>
```
