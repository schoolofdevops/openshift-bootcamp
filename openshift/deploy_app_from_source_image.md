### Deploying apps from Source, Template,  Dockerfile - S2I Builders,Build Configs

#### Launching demo app with source strategy

In this section we are talking about how to deploy application using source. We have already deployed vote frontend application. In this section we are deploying result application and database. We will start with result application which is nodeJs application. First we deploy sample devops demo application which php application for practice. we will deploy this application using oc cli.

```
oc new-app https://github.com/devopsdemoapps/devops-demo-app.git
```
[output]
```
--> Found image 9dd8c80 (2 weeks old) in image stream "openshift/php" under tag "7.1" for "php"

    Apache 2.4 with PHP 7.1
    -----------------------
    PHP 7.1 available as container is a base platform for building and running various PHP 7.1 applications and frameworks. PHP is an HTML-embedded scripting language. PHP attempts to make it easy for developers to write dynamically generated web pages. PHP also offers built-in database integration for several commercial and non-commercial database management systems, so writing a database-enabled webpage with PHP is fairly simple. The most common use of PHP coding is probably as a replacement for CGI scripts.

    Tags: builder, php, php71, rh-php71

    * The source repository appears to match: php
    * A source build using source code from https://github.com/devopsdemoapps/devops-demo-app.git will be created
      * The resulting image will be pushed to image stream "devops-demo-app:latest"
      * Use 'start-build' to trigger a new build
    * This image will be deployed in deployment config "devops-demo-app"
    * Ports 8080/tcp, 8443/tcp will be load balanced by service "devops-demo-app"
      * Other containers can access this service through the hostname "devops-demo-app"

--> Creating resources ...
    imagestream "devops-demo-app" created
    buildconfig "devops-demo-app" created
    deploymentconfig "devops-demo-app" created
    service "devops-demo-app" created
--> Success
    Build scheduled, use 'oc logs -f bc/devops-demo-app' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/devops-demo-app'
    Run 'oc status' to view your app.
```
we need to expose that application using following command.

```
oc expose svc/devops-demo-app
```
[output]
```
route "devops-demo-app" exposed
```

check the status of application

```
oc status
```
[output]

```
In project instavote app (instavote) on server https://128.199.213.193:8443

http://devops-demo-app-instavote.128.199.213.193.nip.io to pod port 8080-tcp (svc/devops-demo-app)
  dc/devops-demo-app deploys istag/devops-demo-app:latest <-
    bc/devops-demo-app source builds https://github.com/devopsdemoapps/devops-demo-app.git on openshift/php:7.1
      build #1 failed about a minute ago - 5eb913a: v2 (Gourav Shah <gs@initcron.org>)
    deployment #1 waiting on image or update

svc/redis - 172.30.57.198:6379
  dc/redis deploys istag/redis:alpine
    deployment #2 deployed 14 minutes ago - 1 pod
    deployment #1 failed 2 hours ago: config change

http://vote-instavote.128.199.213.193.nip.io to pod port 8080 (svc/vote)
  dc/vote deploys istag/vote:v1
    deployment #2 deployed 14 minutes ago - 4 pods
    deployment #1 failed 2 hours ago: config change
```
#### S2I Builder Primer

In this we are talking about how to builds nodeJs application that is result. We are also talking about openshift Builder.

search nodeJs stream in openshift.

```
oc new-app --search nodejs
```
[output]
```
Templates (oc new-app --template=<template>)
-----
nodejs-mongo-persistent
  Project: openshift
  An example Node.js application with a MongoDB database. For more information about using this template, including OpenShift considerations, see https://github.com/openshift/nodejs-ex/blob/master/README.md.

Image streams (oc new-app --image-stream=<image-stream> [--code=<source>])
-----
nodejs
  Project: openshift
  Tags:    6, 8, latest
```
```
oc get is
```
[ouput]

```
NAME              DOCKER REPO                                 TAGS      UPDATED
redis             172.30.1.1:5000/instavote/redis             alpine    22 hours ago
vote              172.30.1.1:5000/instavote/vote              v1        23 hours ago
```
list of image stream using namespace openshift

```
oc get is -n openshift
```
[output]
```
NAME         DOCKER REPO                            TAGS                           UPDATED
dotnet       172.30.1.1:5000/openshift/dotnet       2.0,latest                     11 days ago
httpd        172.30.1.1:5000/openshift/httpd        2.4,latest                     11 days ago
jenkins      172.30.1.1:5000/openshift/jenkins      1,2,latest                     11 days ago
mariadb      172.30.1.1:5000/openshift/mariadb      latest,10.1,10.2               11 days ago
mongodb      172.30.1.1:5000/openshift/mongodb      3.2,3.4,latest + 2 more...     11 days ago
mysql        172.30.1.1:5000/openshift/mysql        5.5,5.6,5.7 + 1 more...        11 days ago
nginx        172.30.1.1:5000/openshift/nginx        1.10,1.12,1.8 + 1 more...      11 days ago
nodejs       172.30.1.1:5000/openshift/nodejs       0.10,4,6 + 2 more...           11 days ago
perl         172.30.1.1:5000/openshift/perl         5.16,5.20,5.24 + 1 more...     11 days ago
php          172.30.1.1:5000/openshift/php          5.5,5.6,7.0 + 2 more...        11 days ago
postgresql   172.30.1.1:5000/openshift/postgresql   9.6,latest,9.2 + 2 more...     11 days ago
python       172.30.1.1:5000/openshift/python       2.7,3.3,3.4 + 3 more...        11 days ago
redis        172.30.1.1:5000/openshift/redis        latest,3.2                     11 days ago
ruby         172.30.1.1:5000/openshift/ruby         2.3,2.4,2.5 + 3 more...        11 days ago
wildfly      172.30.1.1:5000/openshift/wildfly      latest,10.0,10.1 + 4 more...   11 days ago
```
#### Deploying results app with Nodejs S2I Builder

In the section we are deploy node application using custom builder from the images stream nodeJs and tag 8. Search nodejs image using following command.

```
oc new-app --search nodejs
```
[output]
```
Templates (oc new-app --template=<template>)
-----
nodejs-mongo-persistent
  Project: openshift
  An example Node.js application with a MongoDB database. For more information about using this template, including OpenShift considerations, see https://github.com/openshift/nodejs-ex/blob/master/README.md.

Image streams (oc new-app --image-stream=<image-stream> [--code=<source>])
-----
nodejs
  Project: openshift
  Tags:    6, 8, latest
```
Then, Deploy the result application.

```
oc new-app nodejs:8~https://github.com/initcron/example-voting-app.git --context-dir='result' --name=result
```
[output]

```
--> Found image a603093 (2 weeks old) in image stream "openshift/nodejs" under tag "8" for "nodejs:8"

    Node.js 8
    ---------
    Node.js 8 available as container is a base platform for building and running various Node.js 8 applications and frameworks. Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

    Tags: builder, nodejs, nodejs8

    * A source build using source code from https://github.com/initcron/example-voting-app.git will be created
      * The resulting image will be pushed to image stream "result:latest"
      * Use 'start-build' to trigger a new build
    * This image will be deployed in deployment config "result"
    * Port 8080/tcp will be load balanced by service "result"
      * Other containers can access this service through the hostname "result"

--> Creating resources ...
    imagestream "result" created
    buildconfig "result" created
    deploymentconfig "result" created
    service "result" created
--> Success
    Build scheduled, use 'oc logs -f bc/result' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/result'
    Run 'oc status' to view your app.
```
Then, Expose the services
```
oc expose svc/result
```
[output]
```
route "result" exposed
```
 show the build in oc including

```
oc get bc
```

[output]
```
NAME              TYPE      FROM      LATEST
devops-demo-app   Source    Git       1
result            Source    Git       1
```
#### Launching db with a template and parameters
In this section we are talking about result application database. We are connecting database to the result application. Database is postgresql which result connect.
We will use the Template for postgresql. Just search postgresql database  on openshift using following command.

```
oc new-app --search postgres
```   
[output]
```
Templates (oc new-app --template=<template>)
-----
postgresql-persistent
  Project: openshift
  PostgreSQL database service, with persistent storage. For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.

NOTE: Scaling to more than one replica is not supported. You must have persistent volumes available in your cluster to use this template.

Image streams (oc new-app --image-stream=<image-stream> [--code=<source>])
-----
postgresql
  Project: openshift
  Tags:    9.5, 9.6, latest

Docker images (oc new-app --docker-image=<docker-image> [--code=<source>])
-----
postgres
  Registry: Docker Hub
  Tags:     latest
```
The , See available template in openshit.

```
oc get templates -n openshift
```
[output]
```
NAME                       DESCRIPTION                                                                        PARAMETERS        OBJECTS
cakephp-mysql-persistent   An example CakePHP application with a MySQL database. For more information ab...   20 (4 blank)      9
dancer-mysql-persistent    An example Dancer application with a MySQL database. For more information abo...   17 (5 blank)      9
django-psql-persistent     An example Django application with a PostgreSQL database. For more informatio...   20 (5 blank)      9
jenkins-ephemeral          Jenkins service, without persistent storage....                                    7 (all set)       6
jenkins-pipeline-example   This example showcases the new Jenkins Pipeline integration in OpenShift,...       16 (4 blank)      8
mariadb-persistent         MariaDB database service, with persistent storage. For more information about...   9 (3 generated)   4
mongodb-persistent         MongoDB database service, with persistent storage. For more information about...   9 (3 generated)   4
mysql-persistent           MySQL database service, with persistent storage. For more information about u...   9 (3 generated)   4
nodejs-mongo-persistent    An example Node.js application with a MongoDB database. For more information...    19 (4 blank)      9
postgresql-persistent      PostgreSQL database service, with persistent storage. For more information ab...   8 (2 generated)   4
rails-pgsql-persistent     An example Rails application with a PostgreSQL database. For more information...   21 (4 blank)      9
```
describe postgres templates

```
oc describe template postgresql-persistent -n openshift
```
[output]
```
Name:		postgresql-persistent
Namespace:	openshift
Created:	11 days ago
Labels:		<none>
Description:	PostgreSQL database service, with persistent storage. For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.

		NOTE: Scaling to more than one replica is not supported. You must have persistent volumes available in your cluster to use this template.
Annotations:	iconClass=icon-postgresql
		kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"template.openshift.io/v1","kind":"Template","labels":{"template":"postgresql-persistent-template"},"message":"The following service(s) have been created in your project: ${DATABASE_SERVICE_NAME}.\n\n       Username: ${POSTGRESQL_USER}\n       Password: ${POSTGRESQL_PASSWORD}\n  Database Name: ${POSTGRESQL_DATABASE}\n Connection URL: postgresql://${DATABASE_SERVICE_NAME}:5432/\n\nFor more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.","metadata":{"annotations":{"description":"PostgreSQL database service, with persistent storage. For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.\n\nNOTE: Scaling to more than one replica is not supported. You must have persistent volumes available in your cluster to use this template.","iconClass":"icon-postgresql","openshift.io/display-name":"PostgreSQL","openshift.io/documentation-url":"https://docs.openshift.org/latest/using_images/db_images/postgresql.html","openshift.io/long-description":"This template provides a standalone PostgreSQL server with a database created.  The database is stored on persistent storage.  The database name, username, and password are chosen via parameters when provisioning this service.","openshift.io/provider-display-name":"Red Hat, Inc.","openshift.io/support-url":"https://access.redhat.com","tags":"database,postgresql"},"name":"postgresql-persistent","namespace":"openshift"},"objects":[{"apiVersion":"v1","kind":"Secret","metadata":{"annotations":{"template.openshift.io/expose-database_name":"{.data['database-name']}","template.openshift.io/expose-password":"{.data['database-password']}","template.openshift.io/expose-username":"{.data['database-user']}"},"name":"${DATABASE_SERVICE_NAME}"},"stringData":{"database-name":"${POSTGRESQL_DATABASE}","database-password":"${POSTGRESQL_PASSWORD}","database-user":"${POSTGRESQL_USER}"}},{"apiVersion":"v1","kind":"Service","metadata":{"annotations":{"template.openshift.io/expose-uri":"postgres://{.spec.clusterIP}:{.spec.ports[?(.name==\"postgresql\")].port}"},"name":"${DATABASE_SERVICE_NAME}"},"spec":{"ports":[{"name":"postgresql","nodePort":0,"port":5432,"protocol":"TCP","targetPort":5432}],"selector":{"name":"${DATABASE_SERVICE_NAME}"},"sessionAffinity":"None","type":"ClusterIP"},"status":{"loadBalancer":{}}},{"apiVersion":"v1","kind":"PersistentVolumeClaim","metadata":{"name":"${DATABASE_SERVICE_NAME}"},"spec":{"accessModes":["ReadWriteOnce"],"resources":{"requests":{"storage":"${VOLUME_CAPACITY}"}}}},{"apiVersion":"v1","kind":"DeploymentConfig","metadata":{"annotations":{"template.alpha.openshift.io/wait-for-ready":"true"},"name":"${DATABASE_SERVICE_NAME}"},"spec":{"replicas":1,"selector":{"name":"${DATABASE_SERVICE_NAME}"},"strategy":{"type":"Recreate"},"template":{"metadata":{"labels":{"name":"${DATABASE_SERVICE_NAME}"}},"spec":{"containers":[{"capabilities":{},"env":[{"name":"POSTGRESQL_USER","valueFrom":{"secretKeyRef":{"key":"database-user","name":"${DATABASE_SERVICE_NAME}"}}},{"name":"POSTGRESQL_PASSWORD","valueFrom":{"secretKeyRef":{"key":"database-password","name":"${DATABASE_SERVICE_NAME}"}}},{"name":"POSTGRESQL_DATABASE","valueFrom":{"secretKeyRef":{"key":"database-name","name":"${DATABASE_SERVICE_NAME}"}}}],"image":" ","imagePullPolicy":"IfNotPresent","livenessProbe":{"exec":{"command":["/usr/libexec/check-container","--live"]},"initialDelaySeconds":120,"timeoutSeconds":10},"name":"postgresql","ports":[{"containerPort":5432,"protocol":"TCP"}],"readinessProbe":{"exec":{"command":["/usr/libexec/check-container"]},"initialDelaySeconds":5,"timeoutSeconds":1},"resources":{"limits":{"memory":"${MEMORY_LIMIT}"}},"securityContext":{"capabilities":{},"privileged":false},"terminationMessagePath":"/dev/termination-log","volumeMounts":[{"mountPath":"/var/lib/pgsql/data","name":"${DATABASE_SERVICE_NAME}-data"}]}],"dnsPolicy":"ClusterFirst","restartPolicy":"Always","volumes":[{"name":"${DATABASE_SERVICE_NAME}-data","persistentVolumeClaim":{"claimName":"${DATABASE_SERVICE_NAME}"}}]}},"triggers":[{"imageChangeParams":{"automatic":true,"containerNames":["postgresql"],"from":{"kind":"ImageStreamTag","name":"postgresql:${POSTGRESQL_VERSION}","namespace":"${NAMESPACE}"},"lastTriggeredImage":""},"type":"ImageChange"},{"type":"ConfigChange"}]},"status":{}}],"parameters":[{"description":"Maximum amount of memory the container can use.","displayName":"Memory Limit","name":"MEMORY_LIMIT","required":true,"value":"512Mi"},{"description":"The OpenShift Namespace where the ImageStream resides.","displayName":"Namespace","name":"NAMESPACE","value":"openshift"},{"description":"The name of the OpenShift Service exposed for the database.","displayName":"Database Service Name","name":"DATABASE_SERVICE_NAME","required":true,"value":"postgresql"},{"description":"Username for PostgreSQL user that will be used for accessing the database.","displayName":"PostgreSQL Connection Username","from":"user[A-Z0-9]{3}","generate":"expression","name":"POSTGRESQL_USER","required":true},{"description":"Password for the PostgreSQL connection user.","displayName":"PostgreSQL Connection Password","from":"[a-zA-Z0-9]{16}","generate":"expression","name":"POSTGRESQL_PASSWORD","required":true},{"description":"Name of the PostgreSQL database accessed.","displayName":"PostgreSQL Database Name","name":"POSTGRESQL_DATABASE","required":true,"value":"sampledb"},{"description":"Volume space available for data, e.g. 512Mi, 2Gi.","displayName":"Volume Capacity","name":"VOLUME_CAPACITY","required":true,"value":"1Gi"},{"description":"Version of PostgreSQL image to be used (9.4, 9.5, 9.6 or latest).","displayName":"Version of PostgreSQL Image","name":"POSTGRESQL_VERSION","required":true,"value":"9.6"}]}

	openshift.io/display-name=PostgreSQL
	openshift.io/documentation-url=https://docs.openshift.org/latest/using_images/db_images/postgresql.html
	openshift.io/long-description=This template provides a standalone PostgreSQL server with a database created.  The database is stored on persistent storage.  The database name, username, and password are chosen via parameters when provisioning this service.
	openshift.io/provider-display-name=Red Hat, Inc.
	openshift.io/support-url=https://access.redhat.com
	tags=database,postgresql

Parameters:
    Name:		MEMORY_LIMIT
    Display Name:	Memory Limit
    Description:	Maximum amount of memory the container can use.
    Required:		true
    Value:		512Mi

    Name:		NAMESPACE
    Display Name:	Namespace
    Description:	The OpenShift Namespace where the ImageStream resides.
    Required:		false
    Value:		openshift

    Name:		DATABASE_SERVICE_NAME
    Display Name:	Database Service Name
    Description:	The name of the OpenShift Service exposed for the database.
    Required:		true
    Value:		postgresql

    Name:		POSTGRESQL_USER
    Display Name:	PostgreSQL Connection Username
    Description:	Username for PostgreSQL user that will be used for accessing the database.
    Required:		true
    Generated:		expression
    From:		user[A-Z0-9]{3}

    Name:		POSTGRESQL_PASSWORD
    Display Name:	PostgreSQL Connection Password
    Description:	Password for the PostgreSQL connection user.
    Required:		true
    Generated:		expression
    From:		[a-zA-Z0-9]{16}

    Name:		POSTGRESQL_DATABASE
    Display Name:	PostgreSQL Database Name
    Description:	Name of the PostgreSQL database accessed.
    Required:		true
    Value:		sampledb

    Name:		VOLUME_CAPACITY
    Display Name:	Volume Capacity
    Description:	Volume space available for data, e.g. 512Mi, 2Gi.
    Required:		true
    Value:		1Gi

    Name:		POSTGRESQL_VERSION
    Display Name:	Version of PostgreSQL Image
    Description:	Version of PostgreSQL image to be used (9.4, 9.5, 9.6 or latest).
    Required:		true
    Value:		9.6


Object Labels:	template=postgresql-persistent-template

Message:	The following service(s) have been created in your project: ${DATABASE_SERVICE_NAME}.

		       Username: ${POSTGRESQL_USER}
		       Password: ${POSTGRESQL_PASSWORD}
		  Database Name: ${POSTGRESQL_DATABASE}
		 Connection URL: postgresql://${DATABASE_SERVICE_NAME}:5432/

		For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.

Objects:
    Secret			${DATABASE_SERVICE_NAME}
    Service			${DATABASE_SERVICE_NAME}
    PersistentVolumeClaim	${DATABASE_SERVICE_NAME}
    DeploymentConfig		${DATABASE_SERVICE_NAME}
```

```
oc new-app --name=db --template=postgresql-persistent -p DATABASE_SERVICE_NAME=db -p POSTGRESQL_USER=postgres -p POSTGRESQL_PASSWORD=password -p POSTGRESQL_DATABASE=postgres
```
[output]

```
--> Deploying template "openshift/postgresql-persistent" to project instavote

     PostgreSQL
     ---------
     PostgreSQL database service, with persistent storage. For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.

     NOTE: Scaling to more than one replica is not supported. You must have persistent volumes available in your cluster to use this template.

     The following service(s) have been created in your project: db.

            Username: postgres
            Password: password
       Database Name: postgres
      Connection URL: postgresql://db:5432/

     For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.

     * With parameters:
        * Memory Limit=512Mi
        * Namespace=openshift
        * Database Service Name=db
        * PostgreSQL Connection Username=postgres
        * PostgreSQL Connection Password=password
        * PostgreSQL Database Name=postgres
        * Volume Capacity=1Gi
        * Version of PostgreSQL Image=9.6

--> Creating resources ...
    secret "db" created
    service "db" created
    persistentvolumeclaim "db" created
    deploymentconfig "db" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/db'
    Run 'oc status' to view your app.
```
#### Using docker strategy with oc new-app to deploy worker app

In this section we are talking about docker Registry with oc new-app to deploy worker application. We have already deployed vote, result and db application. worker application are java application. Use the following command.

```
oc new-app --name=worker https://github.com/initcron/example-voting-app.git --context-dir=worker
```
[output]
```
error: multiple images or templates matched "jee": 7

The argument "jee" could apply to the following Docker images, OpenShift image streams, or templates:

* Image stream "wildfly" (tag "12.0") in project "openshift"
  Use --image-stream="openshift/wildfly:12.0" to specify this image or template

* Image stream "wildfly" (tag "8.1") in project "openshift"
  Use --image-stream="openshift/wildfly:8.1" to specify this image or template

* Image stream "wildfly" (tag "9.0") in project "openshift"
  Use --image-stream="openshift/wildfly:9.0" to specify this image or template

* Image stream "wildfly" (tag "latest") in project "openshift"
  Use --image-stream="openshift/wildfly:latest" to specify this image or template

* Image stream "wildfly" (tag "10.0") in project "openshift"
  Use --image-stream="openshift/wildfly:10.0" to specify this image or template

To view a full list of matches, use 'oc new-app -S jee'
```

```
oc new-app --name=worker https://github.com/initcron/example-voting-app.git#openshift --context-dir=worker

```
[output]
```
--> Found Docker image 991f779 (22 months old) from Docker Hub for "schoolofdevops/maven"

    * An image stream will be created as "maven:latest" that will track the source image
    * A Docker build using source code from https://github.com/initcron/example-voting-app.git#openshift will be created
      * The resulting image will be pushed to image stream "worker:latest"
      * Every time "maven:latest" changes a new build will be triggered
    * This image will be deployed in deployment config "worker"
    * The image does not expose any ports - if you want to load balance or send traffic to this component
      you will need to create a service with 'expose dc/worker --port=[port]' later
    * WARNING: Image "schoolofdevops/maven" runs as the 'root' user which may not be permitted by your cluster administrator

--> Creating resources ...
    imagestream "maven" created
    imagestream "worker" created
    buildconfig "worker" created
    deploymentconfig "worker" created
--> Success
    Build scheduled, use 'oc logs -f bc/worker' to track its progress.
    Run 'oc status' to view your app.
```
#### Rebuilding from changes to local repository
In the previous section we deployed java worker application but database not connected . In this section we will rebuild application. use  following command for start build.

```
oc start-build worker --from=example-voteing-app
```
