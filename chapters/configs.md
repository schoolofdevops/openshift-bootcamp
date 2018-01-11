
In this lesson we are going to cover the following topics

  * Logging in
  * Disaply Config
  * Create a project


## Logging in

```
oc login

```
## Listing Configurations

Check current config
```
oc get projects
oc status
```


## Creating a project for voting app

Projects offers separation of resources running on the same physical infrastructure into virtual clusters. It is typically useful in mid to large scale environments with multiple projects, teams and need separate scopes.

Lets create a namespace called **vote**  


```
oc new-project voting
```

To create namespace

```
Now using project "voting" on server "https://192.168.64.2:8443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git

to build a new example application in Ruby.
```
