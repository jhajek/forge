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
# type exit to exit the interactive shell
```

### Creating the Jenkins Project for your Android Application

Now lets log into our Jenkins console again. Jenkins works on the concepts of `Jobs` which is essentially a project, but a project with an outcome. You could say you have a **job** to build an Android App from said source code.

The steps to take are as follows:

1) Create `New Item`
1) Give the project a name -- this is for the human so be as specific as you can
1) Select project type of: `Multi-branch`

The new job (item) has been created. Let's configure it. You are automatically taken to the job configuration screen.

1) Give the project a name - this will be a local directory on the system where cloned code is stored so best **NOT** to put spaces in this name
1) Fill in a good description of the project and who is involved in using it
1) Select the `Branch Sources` and choose `GitHub` as the source option
1) Select the Add button and add credentials under the project name (not Jenkins)
1) You will be prompted for a username, password and ID to connect to GitHub. this will be your GitHub user ID, the password will be the GITHUB_TOKEN you created previously and the ID is a note your will create to identify this set of credentials.
1) GitHub Tokens are set to expire every 30 days by default -- at that time you will need to repeat these steps and create new credential pairs (so a good ID is important otherwise you will get confused which tokens are active!)
1) Add `Repository URL` as the full URL to your GitHub repo
1) Make sure to have a Jenkinsfile in the root -- pointing to your project (or a sub-directory if you have one) -- more on this below

### Jenkinsfile

If you are missing a [Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/ "Jenkinsfile documentation webpage") in the root of your repo we need to create one. A `Jenkinsfile` can be placed in locations other than the root of your repo, and you would adjust that path in the Jenkins job configuration. For now we will leave this in the root of the repo.

The syntax for a Jenkinsfile will look something like this:

```bash
// Jenkinsfile template (must have capital "J")
// https://www.jenkins.io/doc/book/pipeline/jenkinsfile/
pipeline {
    agent any

stages {
    stage('Build')  {
          steps {      
                dir("itmd-555/Sample-For-Jenkins") {
                sh 'chmod +x ./gradlew'
                sh './gradlew assembleRelease'
                }
        }
    }    
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Android APK to GitHub Release Tab...'
                    sh '/var/jenkins_home/sample-app-release-script.sh'
                }
            }
        }
    }
}
```

Note the `dir()` which tells the Jenkins server where my Android Gradle project files are located. In this case in a sub-folder `ITMD-555/Sample-For-Jenkins`. Once complete your project will automatically be cloned by Jenkins to the build server and will try to use the Android SDK build tools. [Gradle](https://developer.android.com/studio/releases/gradle-plugin "Android Gradle Plugin webpae") is the tool Android uses to build its projects. It is not an Android or Google tool, but is used by many people for building large software projects.

### Why Your First Build Failed

Your first build will fail with an error message stating that certain tools are not installed that Android needs to build. This sample project assumes I am using a new default Android Studio project using the Android 32 build tools.

```bash
> Failed to install the following Android SDK packages as some licences have not been accepted.
     patcher;v4 SDK Patch Applier v4
     platform-tools Android SDK Platform-Tools
     emulator Android Emulator
     build-tools;30.0.3 Android SDK Build-Tools 30.0.3
     platforms;android-32 Android SDK Platform 32
     tools Android SDK Tools
```

How can we install these missing tools? Normally your Gradle build script is smart enough to go an fetch all the needed build tools and dependencies for you. In this case we are not developing in Android Studio, but developing with the Android SDK tools from the commandline. We need to go back to the Docker container commandline.

```bash
# Use the docker exec -it command
sudo docker exex -it jenkins-fall-class bash
# Change directory into the android-sdk/latest/bin directory to 
# use the sdkmanager
cd ./android-sdk/latest/bin
# Use the sdkmanager to install and accept licenses dependencies
./sdkmanager --install "patcher;v4"
./sdkmanager --install "platform-tools"
./sdkmanager --install "build-tools;30.0.3"
./sdkmanager --install "platforms;android-32"
# Feel free to add any other packages you think you will need for building your software
```

### Accepting Licenses

The reason we have to manually install the additional software packages is due with the fact that you need to manually accept Android SDK licenses before install. [There is another way](https://developer.android.com/studio/intro/update.html#download-with-gradle "Android Studio License Accept alternative method webpage") were you can copy a `license` directory from a system running Android Studio, where you have already accepted the licenses via the `SDK Manager`. Place this `licenses` directory in your `$ANDROID_HOME` directory.



Place the script: sample-app-release.sh into your /var/jenkins_home mapped directory