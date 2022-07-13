---
layout: post
status: publish
published: true
title: 'How to Install and Configure the Android SDK inside of the Official Jenkins Docker Container'
tags: 
- jenkins
- docker
- android
- howto
comments: []
---

Recently I have been interested in creating systems that can be used to compile Android applications. But not just a simple commandline tool installation, but connected to an installation of Jenkins to make a continuous integration pipeline. This will compile your Android code and attach the compuled SDK to the `Release Tab` on your GitHub repo. This doesn't come for free, but with a little work this becomes very do-able.

## Getting Started - what we will need

This article assumes you are using a Linux distro (I prefer Ubuntu), and a [recent installation of Docker](https://docs.docker.com/engine/install/ "Docker community install"). Using the Docker Hub you can find official Docker images for [Jenkins.io](https://hub.docker.com/r/jenkins/jenkins "Jenkins Official Docker Image"). This links to the new official lts docker container `jenkins/jenkins:lts`. This has Jenkins already installed and configured with the latest lts as well as Java 11. You will also need to get the [Android SDK commandline tools](https://developer.android.com/studio/command-line/sdkmanager "Android Commandline Tools webpage") from Google onto your local system. The next steps are to retreive the Jenkins container and start an instance of this container.

```bash
# From the commandline to install the jenkins lts container in Docker
sudo docker pull jenkins/jenkins:lts
# For YOUR-NAME-FOR-THE-CONTAINER I will use "jenkins-fall-class"
docker run -p 8080:8080 -p 50000:50000 --restart=on-failure -v jenkins_home:/var/jenkins_home -name YOUR-NAME-FOR-THE-CONTAINER jenkins/jenkins:lts-jdk11
```

The `docker run` command will do a few things:

* `-p 8080:8080` will expose port 8080 to the public internet and bridge it to port 8080 on the internal Docker network
* `-p 50000:50000` opens up an additional port if you attach build workers to a main build server
  * If you don't plan to have worker nodes, then you can remove this
* `-v jenkins_home:/var/jenkins_home` this is a volume mount command to map a volume/directory from your system into the container as `/var/jenkins_home`
  * This is a way to get files into your container
  * This will be needed later to get the Android commandline-tools zip into the container

## Configure Android inside of the Jenkins Container

To proceed we will need to get a bash commandline inside the container.

```bash
# This command gives you an interactive commandline shell to the container
sudo docker exec -it jenkins-fall-class bash
```

From inside the Jenkins Docker container, as the root, run the following commands:

```bash
cd ~
# We will unzip the commandline tools zip file we provided in the volume mount
unzip commandlinetools-linux-8512546_latest.zip
# Upon succesful unzip -- execute the sdkmanager and give it the
# --sdk_root - where the extracted tools will live
./cmdline-tools/bin/sdkmanager --sdk_root=/var/jenkins_home/android_sdk --install "cmdline-tools;latest"
# I chose /var/jenkins_home/android_sdk, location is arbitrary,
# but should be somewhere the user jenkins has access
```

This will complete the configuration from the command line for now

## Jenkins Web Console and Android Configuration

Now we will work from the Jenkins web console to configure the build server. Jenkins is easily configurable, but being a build-server, it doesn't come with any of the software needed to build software. So we will have to install somethings.

Let's log into the Jenkins console. You will need your public IP address first to log in. In my case my public IP is: `192.168.0.1`. Jenkins runs on port 8080 and is `http` by default. Open a browser on your system and browse to `http://192.168.0.1:8080`.

The first screen you will see will ask you for an administrator password to unlock Jenkins and complete the initial install. You will find the key at `/var/jenkins_home/secrets/initialAdminPassword` or you can find it from the console by looking in the Docker Logs and you will see the unlock key.

```bash
# In this case jenkins-fall-class, is the name of my Docker container instance
sudo docker logs jenkins-fall-class
```

Complete the install and select the default recommended plugins. This will take a few minutes to complete and fully install the default set of plugins. Once this completes you will be presented with the main management page. The next step is to create a Jenkins project.  First step on the Jenkins Dashboard -> Manage Jenkins -> Configure System -> Global Properties, scroll down and select ENV variables.

You will end up adding 3 ENV variables:

* The first variable to add: `GITHUB_TOKEN`
  * You will need to create this under your GitHub account, under account settings
  * These tokens expire after 30 days by default, plan accordingly
* The second variable to add: `ANDROID_SDK`
  * Put the path to the installed sdkmanager, Android needs this to install addtional tools
  * `/var/jenkins_home/android_sdk/cmdline-tools/latest/bin`
* The third variable to add: `ANDROID_HOME`
  * Put the path to the home directory of the sdkmanager
  * `/var/jenkins_home/android_sdk`

## Secure Access Tokens

Back to the commandline of your Docker Jenkins container. You will need to generate an SSH Public/Private keypair to give your container permissions to clone code from your private repo over SSH. We will be generating [Twisted Edwards Curve - ed25519](https://en.wikipedia.org/wiki/Twisted_Edwards_curve "Wikipage for Twisted Edwards Curve pair") keys.

```bash
# This command gives you an interactive commandline shell to the container
sudo docker exec -it jenkins-fall-class bash
# From inside the Docker container and hit enter to the following questions
# and accept the defaults
cd ~
ssh-keygen -t ed25519
# Display the content of the public key just generated
cd .ssh
cat ./id_ed25519.pub
# Copy this content to your clipboard
# Copy 
# Exit the remote shell
```

With the key pair we created in the Docker container we will now be be able to clone our project code via SSH.  You will need to add the content of the Public key (.pub) to GitHub as a [Deploy Key](https://github.blog/2015-06-16-read-only-deploy-keys/ "GitHUb deploy key web page"). GitHub defines a Deploy Key as follows, *"A deploy key is an SSH key that is stored on your server and grants access to a single GitHub repository.*"  Deploy keys are powerful in that there scope can be limited to a single repo and read-only state as they are used just for deploying code from a GitHub repo.

Assuming you have `admin` access to your GitHub account, you can use the repo `Settings` link, then under the `Security` tab select `Deploy keys`.  You will be presented with a list of your keys if you have existing ones, or the screen will be empty if this is your first one. To start click the `Add Deploy Key` button, giving the key a name and pasting the `.pub` or public key content into the text box and save it. Now GitHub has your public key which makes cloning safer--no need to embed or hardcode passwords into your code.

### Install the Hub Release Package

The next step is we have to add the [Hub release application](https://github.com/github/hub/ "Hub web page") to our container.  This package is used to wrap and extend the git binary. Allowing you to push your build artifacts and attach them to your private repo's `Release Tab`.  This is where pre-compiled downloadable binaries can be placed for a user--while maintaining a library of previous versions.

To complete this is Docker, exit the previous interactive shell, if you haven't already, and lets log back in as root to install the HUB package.

```bash
# Remember to log in as the root user, you need to add -u root
# jenkins-fall-class is the name of this container
sudo docker exec -u root -it jenkins-fall-class bash 
# Once at the root prompt at the interactive shell
apt update
apt install hub
```


Jenkins: Create new job > name project > Multi-branch project
Name Project - give description
Branch Sources (select Project Name)
under username give your GitHub ID and Password is the GitHub token you created (remember they are 30 days expire)
for ID place a meaningful name and I would recommend putting a date so you can know which set of credentials is which once they expire
Give the URL to your illinoistech-itm repo -- make sure to have a Jenkinsfile in the root -- pointing to your project (or a sub-directory if you have one)

From Jenkins Docker container bash interactive shell
use the sdkmanager to install and accept licenses dependencies
./sdkmanager --install 
     patcher;v4 
     platform-tools
     build-tools;30.0.3
     platforms;android-32

This has to due with the fact that you need to manually accept Android SDK licenses - this is one way to do it or take a license folder from an already accepted system and place it in the ANDROID_HOME directory
	https://developer.android.com/studio/intro/update.html#download-with-gradle

Place the script: sample-app-release.sh into your /var/jenkins_home mapped directory