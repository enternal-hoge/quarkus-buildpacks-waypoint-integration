# quarkus-buildpacks-waypoint-integration

I'm trying to run a Quarkus application on Hashicorp Waypoint.

Waypoint seems to depend on buildpacks.

Fortunately, the Quarkus community has released buildpacks.

https://github.com/quarkusio/quarkus-buildpacks

My goal is to easily run the Quarkus application on Waypoint with Docker on my local machine.

This is a personal and experimental project, so you may quit prematurely.

# Prerequiment

- Installed Open JDK 11
- Installed GraalVM JDK 11
- Intalled Docker Desktop
- Installed Hashicorp Waypoint

  Refer https://github.com/enternal-hoge/waypoint-introduction

# Try

## Install pack command

```bash
$ git clone https://github.com/quarkusio/quarkus-buildpacks.git
$ cd quarkus-buildpacks
$ ./create-buildpacks.sh
---> Building Stacks
---> Building Stack native
BUILDING redhat/buildpacks-stack-quarkus-build:native...
Sending build context to Docker daemon  2.048kB
Step 1/11 : FROM quay.io/quarkus/centos-quarkus-maven:21.0.0-java11
21.0.0-java11: Pulling from quarkus/centos-quarkus-maven
7a0437f04f83: Pull complete 
2e038531a7f6: Pull complete 
Digest: sha256:a19243ed3cc3ebb03ef93c56d74fd94eb106e2c0c790af7bd3931d2487e500b2
Status: Downloaded newer image for quay.io/quarkus/centos-quarkus-maven:21.0.0-java11
...

STACK BUILT!

Stack ID: io.quarkus.buildpacks.stack.jvm
Images:
    redhat/buildpacks-stack-quarkus-build:jvm
    redhat/buildpacks-stack-quarkus-run:jvm
---> Creating builder for builder quarkus-native
Downloading from https://github.com/buildpacks/lifecycle/releases/download/v0.10.2/lifecycle-v0.10.2+linux.x86-64.tgz
5.15 MB/5.15 MB
Successfully created builder image redhat/buildpacks-builder-quarkus-native:latest
Tip: Run pack build <image-name> --builder redhat/buildpacks-builder-quarkus-native:latest to use this builder
Builder redhat/buildpacks-builder-quarkus-native:latest is now trusted
---> Creating builder for builder quarkus-jvm
Successfully created builder image redhat/buildpacks-builder-quarkus-jvm:latest
Tip: Run pack build <image-name> --builder redhat/buildpacks-builder-quarkus-jvm:latest to use this builder
Builder redhat/buildpacks-builder-quarkus-jvm:latest is now trusted
```

Confirm docker informations.

```bash
$ docker images
REPOSITORY                                   TAG                 IMAGE ID            CREATED             SIZE
redhat/buildpacks-stack-quarkus-build        jvm                 f314f14e9bca        2 minutes ago       612MB
redhat/buildpacks-stack-quarkus-run          jvm                 54e084e1023e        2 minutes ago       612MB
redhat/buildpacks-stack-quarkus-base         jvm                 a846fc08cfc6        2 minutes ago       612MB
redhat/buildpacks-stack-quarkus-run          native              2d83111d4255        2 minutes ago       103MB
redhat/buildpacks-stack-quarkus-build        native              a384a8a1fc0d        3 minutes ago       1.96GB
...
redhat/buildpacks-builder-quarkus-native     latest              afa208608b5d        41 years ago        1.97GB
redhat/buildpacks-builder-quarkus-jvm        latest              9df5e2eb5a42        41 years ago        626MB
```

## Run Example Application

```bash
$ pack build quarkus-jvm-test-app --path apps/quarkus-sample-app --builder redhat/buildpacks-builder-quarkus-jvm:latest
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  04:25 min
[INFO] Finished at: 2021-05-02T03:26:21Z
[INFO] ------------------------------------------------------------------------
...
*** Images (1a7170f82ad1):
      quarkus-jvm-test-app
Adding cache layer 'io.quarkus.buildpacks.buildpack:m2repo'
Successfully built image quarkus-jvm-test-app
```

```
$ docker run -it --rm quarkus-jvm-test-app                                    
INFO exec  java -XX:+UseParallelOldGC -XX:MinHeapFreeRatio=10 -XX:MaxHeapFreeRatio=20 -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -XX:MaxMetaspaceSize=100m -XX:+ExitOnOutOfMemoryError -cp ".:target/quarkus-app/lib" -jar /layers/io.quarkus.buildpacks.buildpack/1-app/quarkus-run.jar  
__  ____  __  _____   ___  __ ____  ______ 
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
2021-05-02 03:30:07,640 INFO  [io.quarkus] (main) quarkus-sample-app 1.0.0-SNAPSHOT on JVM (powered by Quarkus 1.13.0.Final) started in 1.174s. Listening on: http://0.0.0.0:8080
2021-05-02 03:30:07,680 INFO  [io.quarkus] (main) Profile prod activated. 
2021-05-02 03:30:07,680 INFO  [io.quarkus] (main) Installed features: [cdi, resteasy]
```

## TBD

In Progress..

