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
