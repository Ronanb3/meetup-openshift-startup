# Lab 2 : Connect to Openshift and create your first application using the command line

In this lab, we will connect to OpenShift and create an application using the command line

## Command Line Interface (CLI)

The OpenShift CLI is accessed using the command *oc*. From here, you can administrate the entire OpenShift cluster and deploy new applications.

The CLI exposes the underlying Kubernetes orchestration system with the enhancements made by OpenShift. Users familiar with Kubernetes will be able to adapt to OpenShift quickly. *oc* provides all of the functionality of *kubectl*, along with additional functionality to make it easier to work with OpenShift. The CLI is ideal in situations where you are:

1) Working directly with project source code

2) Scripting OpenShift operations

3) Restricted by bandwidth resources and cannot use the web console

## Deploy an application with `oc` from the command line

The OpenShift `oc` command line tool includes all the functionality of the Kubernetes native `kubectl` CLI but it has also all the function required for OpenShift specifics, e.g. a `login` command to access the OpenShift cluster.

Follow instructions in the Prerequisites to log to your OCP  cluster.

### Working with the `oc` CLI

1. Go back to your command line where you used `oc`to logon to your OpenShift cluster.

2. You have already a project created. With this test environment you cannot create a new project.

### Deploy an application from the command line

- Check if the image is available:

```
$ oc new-app --search openshiftkatacoda/blog-django-py

Docker images (oc new-app --docker-image=<docker-image> [--code=<source>])
-----
openshiftkatacoda/blog-django-py
  Registry: Docker Hub
  Tags:     latest
```

- Deploy the image as an application:

```
$ oc new-app openshiftkatacoda/blog-django-py
--> Found Docker image 927f823 (2 months old) from Docker Hub for "openshiftkatacoda/blog-django-py"

    Python 3.5
    ----------
    Python 3.5 available as container is a base platform for building and running various Python 3.5 applications and frameworks. Python is an easy to learn, powerful programming language. It has efficient high-level data structures and a simple but effective approach to object-oriented programming. Python's elegant syntax and dynamic typing, together with its interpreted nature, make it an ideal language for scripting and rapid application development in many areas on most platforms.
    Tags: builder, python, python35, python-35, rh-python35
    * An image stream tag will be created as "blog-django-py:latest" that will track this image
    * This image will be deployed in deployment config "blog-django-py"    
    * Port 8080/tcp will be load balanced by service "blog-django-py"      
        * Other containers can access this service through the hostname "blog-django-py"
        
--> Creating resources ...
    imagestream.image.openshift.io "blog-django-py" created    
    deploymentconfig.apps.openshift.io "blog-django-py" created    
    service "blog-django-py" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/blog-django-py'
     Run 'oc status' to view your app.
```

- Check the status of your deployment:

```
$ oc status --suggest

In project blog on server https://c100-e.us-south.containers.cloud.ibm.com:30634
svc/blog-django-py - 172.21.51.187:8080
  dc/blog-django-py deploys istag/blog-django-py:latest
     deployment #1 deployed 56 seconds ago - 1 pod

Info:
  * dc/blog-django-py has no readiness probe to verify pods are ready to accept traffic or ensure deployment is successful.
    try: oc set probe dc/blog-django-py --readiness ...
  * dc/blog-django-py has no liveness probe to verify pods are still running.
    try: oc set probe dc/blog-django-py --liveness ...
```

The --suggest options even gives you infos on things that are missing in your configuration.

- Your application needs a Route to expose it externally:

```
$ oc expose service/blog-django-py

route.route.openshift.io/blog-django-py exposed
```

- Display the URL of the Route:

```
$ oc get route/blog-django-py
NAME             HOST/PORT                                                                                                               PATH      SERVICES         PORT       TERMINATION   WILDCARD
blog-django-py   blog-django-py-blog.harald-uebele-openshift-5290c8c8e5797924dc1ad5d1bcdb37c0-0001.us-south.containers.appdomain.cloud             blog-django-py   8080-tcp                 None
```

You can see the very long URL. If you want, copy it and open it in your browser.

You just created your first application in OpenShift in command line mode !
