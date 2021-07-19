# k8sScw.com

Setup sancapweather.com in a kubernetes cluster as deployment named scwcom; expose as service scwcom; connect to the internet as sancapweather.com through an ingress called scwcom-ingress. The ambientweather.net server collects the realtime data from the weather station in Sanibel.  The sancapweather.com uses apis from ambientweather to get the realtime data and render it... there is not volume storage for this application.

Source image [InstallSCW.com](https://github.com/jkozik/InstallSCW.com)

# build image, put on docker hub
SanCapWeather.com is running in a docker container on my host, directly -- not in a VM.  To make it run in my kubernetes cluster, I need to push the image to my dockerhub repository.  The kubernetes deployment resource has an "image" field that triggers a pull of this image from dockerhub.
```
[jkozik@dell2 k8sNw.net]$ docker tag jkozik/scw.com jkozik/scw.com:v1
[jkozik@dell2 k8sNw.net]$ docker push jkozik/scw.com:v1
The push refers to a repository [docker.io/jkozik/scw.com]
df6373f27d47: Pushed
26f167811204: Pushed
f3493de6be65: Pushed
5f3f1afd37f8: Pushed
9cb1c8944404: Pushed
17ef7ac3bc3c: Pushed
80e1222a1520: Pushed
84cddc3bfd9f: Pushed
91eac03e1433: Pushed
9fe29cfcdaad: Pushed
c907f065ed59: Mounted from jkozik/nw.com
1920409046ed: Mounted from jkozik/nw.net
1b04f74cccfe: Mounted from jkozik/nw.net
9f80175acb76: Mounted from jkozik/nw.net
4394b53db28c: Mounted from jkozik/nw.net
5b35dc81a1f5: Mounted from jkozik/nw.net
76d251b895a5: Mounted from jkozik/nw.net
7f5982d81e05: Mounted from jkozik/nw.net
ea7c1c005303: Mounted from jkozik/nw.net
df83fc87c7b6: Mounted from jkozik/nw.net
17e81ae0b70b: Mounted from jkozik/nw.net
e80100d0d319: Mounted from jkozik/nw.net
cc0f976c1376: Mounted from jkozik/nw.net
67415c00b86b: Mounted from jkozik/nw.net
ffc9b21953f4: Mounted from jkozik/nw.net
v1: digest: sha256:2ba522ab2d6b7f9ba41dce3d3b47948e8eb3b95c971237c3da726e1d8fc965c1 size: 5554
```
## Deploy
Clone the yaml files in this repository.  One can apply them one at a time, but it works fine to apply them all at once.  On the kubectl host, run the following to deploy all in one command:
```
[jkozik@dell2 ~]$ git clone https://github.com/jkozik/k8sScw.com
Cloning into 'k8sScw.com'...
remote: Enumerating objects: 14, done.
remote: Counting objects: 100% (14/14), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 14 (delta 1), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (14/14), done.

[jkozik@dell2 ~]$ cd k8sScw.com/
[jkozik@dell2 k8sScw.com]$ ls
README.md  scwcom-deploy.yml  scwcom-ingress.yml  scwcom-svc.yml

[jkozik@dell2 k8sScw.com]$ kubectl apply -f .
deployment.apps/scwcom created
ingress.networking.k8s.io/scwcom-ingress created
service/scwcom created
```
## Verify that the application is running
```
[jkozik@dell2 k8sScw.com]kubectl get deployment,service,pod,ingress
NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/scwcom                1/1     1            1           112s

NAME                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/scwcom            NodePort    10.104.191.227   <none>        80:31436/TCP   111s

NAME                                       READY   STATUS    RESTARTS   AGE
pod/scwcom-74dc4585cf-d5ws8                1/1     Running   0          112s

NAME                                                CLASS    HOSTS                     ADDRESS           PORTS   AGE
ingress.networking.k8s.io/scwcom-ingress            <none>   sancapweather.com         192.168.100.174   80      112s
```

