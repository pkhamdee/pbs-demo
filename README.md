# Pivotal Build Service Spring Boot Demo

### example-team.yaml

```
name: example-team-name
registries:
- registry: index.docker.io
  username: pasapples
  password: .....
repositories:
- domain: github.com
  username: papicella
  password: ....
```

### example-image.yaml

```
team: example-team-name
source:
  git:
    url: https://github.com/papicella/pbs-demo
    revision: master
image:
  tag: pasapples/pbs-demo-image
```

### Create Config 

```
pb team apply -f example-team.yaml
pb image apply -f example-image.yaml
```

### View Builds / Logs

```
pb image builds pasapples/pbs-demo-image
pb image logs pasapples/pbs-demo-image -b 1 -f
```

### Pull down image locally and run

```
docker pull pasapples/pbs-demo-image
docker run -p 8080:8080 pasapples/pbs-demo-image
```

### Run in a Kubernetes Cluster

``` yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: pbs-demo-image
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: pbs-demo-image
    spec:
      containers:
        - name: pbs-demo-image
          image: pasapples/pbs-demo-image
          ports:
            - containerPort: 8080

---
apiVersion: v1
kind: Service
metadata:
  name: pbs-demo-image-service
  labels:
    name: pbs-demo-image-service
spec:
  ports:
    - port: 80
      targetPort: 8080
      protocol: TCP
  selector:
    app: pbs-demo-image
  type: LoadBalancer
```

```
$ kubectl create -f pbs-image-k8s-yaml.yaml
```

``` http request
$ http http://10.195.75.148/customers/1
HTTP/1.1 200
Content-Type: application/hal+json;charset=UTF-8
Date: Thu, 22 Aug 2019 00:31:58 GMT
Transfer-Encoding: chunked

{
    "_links": {
        "customer": {
            "href": "http://10.195.75.148/customers/1"
        },
        "self": {
            "href": "http://10.195.75.148/customers/1"
        }
    },
    "name": "pas",
    "status": "active"
}
```


<hr />

Pas Apicella [papicella at pivotal.io] is an Advisory Platform Architect at Pivotal Australia