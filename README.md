# fun-with-contour


Installation
(will install contour and envoy images)
Get the deployment yaml
```
wget https://raw.githubusercontent.com/projectcontour/contour/release-1.13/examples/render/contour.yaml
```
modify the externalTrafficPolicy to 'Cluster'
```
  externalTrafficPolicy: Cluster
```
apply the yaml
```
kubectl -f ./contour.yaml
```

Confirm that an envoy loadbalancer service has been created and an external ip has been assigned to it.
```
get -n projectcontour service envoy -o wide
NAME    TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE     SELECTOR
envoy   LoadBalancer   198.51.100.224   192.168.2.7   80:30014/TCP,443:31389/TCP   5m27s   app=envoy
```

Setup your dns to point to this external ip eg.
```
nslookup contour.vinalhaven.info
Server:		10.21.82.100
Address:	10.21.82.100#53

Non-authoritative answer:
Name:	contour.vinalhaven.info
Address: 192.168.2.7
```

Apply the sample application, and contour httproxy objects
```
kubectl apply -f ./kuard.yaml
```

Access the application at 
```
http://contour.vinalhaven.info
```

![](https://raw.githubusercontent.com/bmullan-pivotal/fun-with-contour/6c3cd9056cf29e3827745878b36b6d9dde2c580b/screenshot.png)


