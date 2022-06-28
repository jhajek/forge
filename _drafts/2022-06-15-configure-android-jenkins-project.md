---
layout: post
status: publish
published: true
title: 'Building Android Projects via a Jenkins Build Server'
tags: 
- Android
- ci/cd
- DevOps
- git
comments: []
---

One of the toughest things to do in life is get code into production. When developing Android APKs you can make use of the excellent [Android Studio](https://developer.android.com/studio "Android Studio webpage").

This works really well for single developers, but as soon as you have multiple developers or multiple testers, coders, and users, there comes a question of how do you distribute the Android binaries? How do you make sure that you have a single source of truth that everyone can deploy -- not just a single laptop with all the bits configured?

## You need Version Control

First Git and GitHub--no brainer for any kind of substantial development.

## You need a CD Server

[What is Continuous Delivery (CD)](https://github.com/cdfoundation/faq#what-is-continuous-delivery-cd "CD definition webpage")?

CD is a software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time. The rise of microservices, cloud native architectures has caused a corollary rise in continuous delivery practices. This is related to CI/CD that includes Continuous Integration (CI) -- the practice of merging all developer working copies to a shared mainline several times a day.

### Jenkins

This can get a bit more complicated as now you have to setup [Jenkins](https://jenkins.io "Jenkins CD Server webpage") which is one of the default CD Servers out there.  There are others, but many start here and then move to other platforms once they outgrow Jenkins. If you are doing development involving more than one person, then Jenkins is required. You can self-host or there are paid Cloud versions available.

See my Jenkins setup post -- here

### Android Commandline tools

You will need the `sdkmanager` for installing Android packages (version compiling tools) which is part of the [`commandline-tools`](https://developer.android.com/studio/command-line#tools-sdk "Android Command Line tools") zip file. You no longer need the `android-sdk` package installed via apt -- that is too old. These commandline-tools run the Android build tools from the commandline via the `Jenkinsfile`.

After you download the cmmdlinetools (I had to download them on windows and sftp the to the Jenkins Server)

* Unzip the contents
* Using the direct path install the tools via the command:
  * ```./cmdline-tools/bin/sdkmanager --install "cmdline-tools;latest" --sdk_root=/home/vagrant/android_sdk```
  * The ```--sdk_root``` is the directory where the new sdk tools will be installed to
* Set the `.bashrc` to add the path to the `sdkmanager` to the system path
* Set the Jenkins global settings to find this path
  * Set `ANDROID_SDK_HOME` to the `sdk_root` you defined earlier
  * On Linux set `JAVA_HOME` to `/usr`

Once everything is set the output of `--list_installed` should look like below. Finally there is a permission issue to consider. In order for `Jenkins` to use the `sdkmanager` and write files -- the `sdkmanager` would need to owned by `jenkins:jenkins` -- but if you want to install packages -- the files should be owned by your user. A few things can be done:

* use `sudo`
* `chown -R jenkins:jenkins` and then back to your own user
* Though there has to be a better way to do this

```bash
sdkmanager --list_installed

######Output
  Path                 | Version      | Description                             | Location
  -------              | -------      | -------                                 | -------
  build-tools;28.0.3   | 28.0.3       | Android SDK Build-Tools 28.0.3          | build-tools/28.0.3
  cmake;3.6.4111459    | 3.6.4111459  | CMake 3.6.4111459                       | cmake/3.6.4111459
  cmdline-tools;latest | 7.0          | Android SDK Command-line Tools (latest) | cmdline-tools/latest
  emulator             | 31.2.10      | Android Emulator                        | emulator
  ndk;20.0.5594570     | 20.0.5594570 | NDK (Side by side) 20.0.5594570         | ndk/20.0.5594570
  patcher;v4           | 1            | SDK Patch Applier v4                    | patcher/v4
  platform-tools       | 33.0.2       | Android SDK Platform-Tools              | platform-tools
```

### Commands

```bash
sdkmanager --install 'cmake;3.6.4111459'
sdkmanager --install 'ndk;20.0.5594570'
sdkmanager --install 'build-tools;28.0.3'
sdkmanager --install 'platforms;android-28'
```

## Jenkins Project

* Choose Multi-pipeline
* Setup credentials
  * (Use Personal Access Token -- add URL and validate)
* Include a file named: `Jenkinsfile` in the root of your Android Project repo.
  * This `Jenkinsfile` will automatically be scanned for by Jenkins
  * Is essentially a template file that will call the commands you need to build you Android Project
    * For Android there is a [commandline sdk](https://developer.android.com/studio/build/building-cmdline "Android SDK cmd tools") that has the build tools

Here is a sample [`Jenkinsfile`](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/ "Sample Jenkinsfile webpage"). The file will execute from the root directory of your repository, will change directory to the location of the `gradlew` file, and then execute your build. Here the build is defined as `assembleRelease` -- it could also be defined `assembleDebug` depending on the life-cycle of the app.

```bash
// https://www.jenkins.io/doc/book/pipeline/jenkinsfile/
pipeline {
    agent any

stages {
    stage('Build')  {
          steps {      
                dir("ap-final-app") {
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
    }
}
```

### The dir directive

In your `Jenkinsfile`, you have to understand [pathing](https://jeremyhajek.com/2022/06/15/what-is-debugging.html).  The `Jenkinsfile` executes in the root directory, but if your `gradlew` executable is in a different directory Jenkins won't find it.  You need to add a `dir()` directive--essentially a `cd` command to get to the right directory to execute your `gradlew` command.  

Without being in the correct directory you can get build errors such as the one below, which lead you on a wild goose chase. The real problem was you were trying to execute something and you were in the wrong directory (the computer doesn't know that).

```java
groovy.lang.MissingPropertyException: No such property: assembleDebug for class: groovy.lang.Binding
  at groovy.lang.Binding.getVariable(Binding.java:63)
  at org.jenkinsci.plugins.scriptsecurity.sandbox.groovy.SandboxInterceptor.onGetProperty(SandboxInterceptor.java:251)
```

### Use Hub for deploying releases

Now that you have successfully built an APK on a build server using [Jenkins](https://jenkins.io "Jenkins webpage"), you need to get this artifact deployed to the `Releases` tab on GitHub. The `Releases` tab allows for a central location to release artifacts that can be downloaded and then side-loaded onto your Android device.

This requires a third part tool from GitHub called: [`Hub`](https://hub.github.com/#scripting] "Hub project website").

### Conclusion

Automation is the key.  Yet Operating systems and compiler tools were not designed for automated creation and deployment of apps.  This is a great opportunity but takes some time to put together.  But once it is together it can speed deployment and get your Android App into you and your development teams hands.
