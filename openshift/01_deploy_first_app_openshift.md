### Deploy First Openshift Application.
Let start to deploy our first sample application on openshift and you don't know have to docker and kubernetes because openshift is pass solution only need for the source code.
* Go to openshift UI

![openshit](images/openshift_home.png)
* Click on myproject
* Go to catalog and select php

![Project](images/app1.png)
* click next
* select the php version
* provide git repo.
```
https://github.com/openshift/cakephp-ex.git
```
* click next and close

![project](images/app2.png)

![project](images/app3.png)

* click Overview and see the builds

![project](images/app5.png)

you can click on bulid it goes to build UI you can see here log, terminal, datails, Events

![project](images/app7.png)

Once you build the image it automatically trigger new Depolyment. 

* Go to Overview

you can see here deployment

![project](images/app8.png)

* Go to Build -> images

you can see here published images

![project](images/app9.png)
