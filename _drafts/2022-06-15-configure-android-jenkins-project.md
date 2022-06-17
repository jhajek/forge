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

## You need a Version Control

First Git and GitHub--no brainer for any kind of substantial development.

## You need a CD Server

[What is Continuous Delivery (CD)](https://github.com/cdfoundation/faq#what-is-continuous-delivery-cd "CD definition webpage")?

CD is a software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time. The rise of microservices, cloud native architectures has caused a corollary rise in continuous delivery practices. This is related to CI/CD that includes Continuous Integration (CI) -- the practice of merging all developer working copies to a shared mainline several times a day.

### Jenkins

This can get a bit more complicated as now you have to setup [Jenkins](https://jenkins.io "Jenkins CD Server webpage") which is one of the default CD Servers out there.  There are others, but many start here and then move to other platforms once they outgrow Jenkins. If you are doing development involving more than one person, then Jenkins is required. You can self-host or there are paid Cloud versions available.

See my Jenkins setup post -- here

## Jenkins Project

Choose Multi pipeline
Setup credentials
  (Use PAT -- add URL and validate)
Include a file named: `Jenkinsfile` in the root of your Android Project repo.
  This `Jenkinsfile` will automatically be scanned for and is essentially a template file that will call the commands you need to build you Android Project
  //# https://developer.android.com/studio/build/building-cmdline

Here is a sample for a repository to build a textbook using the [Pandoc markdown library](https://pandoc.org "Pandoc Markdown Library webpage") and the [github-release](https://github.com/github-release/github-release "GitHub Release tool webpage") tools to post the compiled ePub and PDF under the Repo release tab

```bash
// https://www.jenkins.io/doc/book/pipeline/jenkinsfile/
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                  echo 'Building..'
                  sh 'chmod +x ./build-linux-and-macos.sh'
                  sh './build-linux-and-macos.sh'
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
                    echo 'Deploying....'
                    sh '/opt/github-push-scripts/linux-textbook-release-script.sh'
                }
                
            }
        }
    }
}
```



### Build Error

```java
groovy.lang.MissingPropertyException: No such property: assembleDebug for class: groovy.lang.Binding
  at groovy.lang.Binding.getVariable(Binding.java:63)
  at org.jenkinsci.plugins.scriptsecurity.sandbox.groovy.SandboxInterceptor.onGetProperty(SandboxInterceptor.java:251)
```
