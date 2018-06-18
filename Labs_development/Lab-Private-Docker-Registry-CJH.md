Lab - Push Docker Images to the ICP Private Docker Registry
---

### Table of contents
[1. Overview](#login)

[2. Preparing to Build a Docker Image](#prepthebuild)

[3. Build a Docker Image](#buildanimage)

[4. Prepare to Push the Docker Image to the ICP Private Docker Registry](#prepthepush)

[5. Push a Docker Image to the ICP Private Docker Registry](#pushtheimage)

## Overview <a name="Overview"></a>
In this lab exercise, you use a Java application that is packaged as a WAR file, and build a Docker container that combines the official IBM WebSphere Liberty Docker image with the application WAR file. Then, you log in to the ICP Private Docker Registry, and push the custom Docker image to the registry.

## Preparing to Build a Docker Image <a name="prepthebuild"></a>
You start with an application WAR file (HelloFromLiberty.war) that is already created. This application is very simple, and  contains a single JSP that, when run, prints the message "Hello World from Liberty on IBM Cloud Private" in your browser.

![HelloFromLibery Application](images/privateregistry/Private-Registry-01.png)

Make a directory called "HelloFromLiberty" to hold all of the components that you use to build your Docker image.  For this lab, your new directory will contain only two items:

1. The "HelloFromLiberty.war" WAR file.
2. A Dockerfile.

A Dockerfile is a file that contains instructions for the Docker "build" command that describes the components and the process for building a Docker image.

Copy the [HelloFromLiberty.war](Assets/privateregistry/HelloFromLiberty.war) file into the HelloFromLiberty directory that you just created. Using the editor of your choice, create a file with the name "Dockerfile" that contains the following two lines:

![Dockerfile](images/privateregistry/Private-Registry-03.png)

When you are finished, your directory should look like this:

![HelloFromLiberty Directory](images/privateregistry/Private-Registry-02.png)

Next, you build the Docker image.

## Build a Docker Image <a name="buildanimage"></a>

Make sure that you are in the HelloFromLiberty directory, and then run the Docker "build" command as shown in the following image:

![Build the HelloFromLiberty Docker Image](images/privateregistry/Private-Registry-04.png)

The "-t" option in the above build command instructs Docker to add a "tag" to the image that it builds.  The "." indicates that the Dockerfile to use to build the Docker image is located in the current directory.  After the Docker image is successfully created, you do not see the resulting image in the current directory. Docker build stores the newly created Docker image in the "local" Docker repository.  The "local" Docker repository is a repository that resides on the server on which you execute the Docker build command.

After the build is complete, you can use the Docker "images" command to view the contents of the local Docker repository.

![Local Repository](images/privateregistry/Private-Registry-05.png)

## Prepare to Push the Docker Image to the ICP Private Docker Registry <a name="prepthepush"></a>

Before you can successfully push a Docker image to the ICP Private Docker Registry, there are two things that you must do to prepare:

1. ICP must have a namespace that matches the name of the repository within the registry that you are storing the Docker image in.
2. The Docker image must be prefixed with the URI for the ICP Private Docker Registry.

### Create a Namespace in ICP

Recall from above that when you built the Docker image, the tag that is assigned to it contains the repository name "demoicp".  You musto make sure that ICP has a matching namespace defined. If there is no namespace in ICP that corresponds to the repository in the ICP Private Docker Registry, then you get an authentication error when you attempt to push your Docker image.

Log in to the ICP console as admin/admin, and from the navigation menu, select "Namespaces".

![Console Namespaces](images/privateregistry/Private-Registry-08.png)

Scroll through the list of namespaces defined in ICP, and confirm that there is no namespace with the name "demoicp".  If there is an existing namespace by that name, then you are done with this step, and you can log out.  If, however, there is no namespace with the name "demoicp", create one by clicking "Create Namespace":

![New Namespace](images/privateregistry/Private-Registry-09.png)

In the pop-up window, enter the name of the new namespace, "demoicp," and click "create":

![Create Namespace](images/privateregistry/Private-Registry-10.png)

After the namespace is created, confirm that it appears in the list of available namespaces.

![ICP Namespaces](images/privateregistry/Private-Registry-11.png)

### Add the Registry URI to the Docker Image Tag

To successfully push a Docker image to the ICP Private Docker Registry, the image tag must conform to the correct format, as follows:

	<Registry URI>/<Repository Name>/<Image Name>:<Image Version>
	
The tag that you attached to the Docker image when you created it does not contain the registry URI as a prefix.  Before you can push the Docker image to the registry, you must add another tag to the image. A Docker image can be tagged with an any number of tags, so in this case, so you can add a tag, rather than rename one.  Use the Docker "tag" command to add a tag to the Docker image:

![Add an Image Tag](images/privateregistry/Private-Registry-07.png)

## Push a Docker Image to the ICP Private Docker Registry <a name="pushtheimage"></a>

You created a Docker image, prepared the image for the ICP Private Docker Registry, and prepared ICP to receive the image.  The final two steps in the process are:

1. Authenticate with the ICP Private Docker Registry.
2. Push your Docker image to the registry.

### Authenticate to the ICP Private Docker Registry

To authenticate to the ICP Private Docker Registry, use the Docker "login" command with your ICP console credentials (admin/admin):

![Authenticate to the Registry](images/privateregistry/Private-Registry-06.png)

### Push a Docker Image

Now that everything is ready, and you authenticated to the ICP Private Docker Registry, you can push your Docker image to the registry:

![Push to the Registry](images/privateregistry/Private-Registry-12.png)

To confirm that the new Docker image is successfully pushed to the ICP Private Docker Registry, log in to the ICP console, and confirm that the new image appears in the list of available Docker images. From the menu, select "Manage --> Images"

![Open Image List](images/privateregistry/Private-Registry-13.png)

Your Docker image should appear in the list of available Docker images.

![View Images List](images/privateregistry/Private-Registry-14.png)

Congratulations! You successfully created a Docker image, and installed it in the ICP Private Docker Registry.

## End of Lab Review
  In this lab exercise, you learned how to:
  1. Prepare the directories and files required to build a Docker image.
  2. Build a Docker image with the Docker "build" command.
  2. Prepare ICP to accept a Docker image in the Private Docker Registry.
  3. Authenticate with the ICP Private Docker Registry and push a new Docker image into the registry.

## End of Lab Exercise
