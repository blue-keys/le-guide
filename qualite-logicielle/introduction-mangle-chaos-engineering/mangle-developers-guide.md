# Mangle Developers' Guide

## Sub Modules <a href="#sub-modules" id="sub-modules"></a>

Mangle is a spring-boot application with implementation of web services to invoke Fault injection on Supported Endpoints.

The mangle Code is organised is as below sub modules using Maven build tool.

```
├──  assets/files├──  checkstyle├──  docker├──  docs├──  formatter├──  mangle-byteman-root├──  mangle-default-plugin├──  mangle-metric-reporter├──  mangle-models├──  mangle-services├──  mangle-support├──  mangle-task-framework├──  mangle-test-plugin├──  mangle-ui├──  mangle-utils├──  mangle-vcenter-adapter│    .gitbook.yaml│    .gitignore│    CONTRIBUTING.md│    LICENSE│    NOTICE│    pom.xml
```

### mangle-byteman-root: <a href="#mangle-byteman-root" id="mangle-byteman-root"></a>

Module for the Mangle java Agent, which is used to support application level fault injection against apps running on JVM.

### mangle-models: <a href="#mangle-models" id="mangle-models"></a>

&#x20;Module for the complete data model of Mangle Web Application.

### mangle–utils:  <a href="#mangle-utils" id="mangle-utils"></a>

Module for all the core logic shared across Mangle Application.

### mangle-task-framework:  <a href="#mangle-task-framework" id="mangle-task-framework"></a>

Module for command execution orchestration in Mangle. Every fault execution or request processing in Mangle is handled as an asynchronous task. This module also contains the code responsible for managing scheduler functionality of Mangle.

### mangle-test-plugin:  <a href="#mangle-test-plugin" id="mangle-test-plugin"></a>

Module for the test utilities required for testing the mangle–task-framework.

### mangle-default-plugin:  <a href="#mangle-default-plugin" id="mangle-default-plugin"></a>

Module for all the out of the box faults supported by Mangle as a plugin. This is developed using the pf4j-spring framework. Mangle can support fault execution only if this module is available as plugin.

### mangle-ui <a href="#mangle-ui" id="mangle-ui"></a>

Module for the presentation layer of mangle.

### mangle –services <a href="#mangle-services" id="mangle-services"></a>

Module for core web services exposed to user, corresponding persistence layer and business logic.

### mangle-vcenter-adapter: <a href="#mangle-vcenter-adapter" id="mangle-vcenter-adapter"></a>

Module for the spring-boot application that has to be deployed as a container or hosted as an application reachable by Mangle for executing faults against VMware vCenter Server.

The order of execution of sub modules is:

```
mangle-byteman-rootmangle-modelsmangle-utilsmangle-task-frameworkmangle-test-pluginmangle-default-pluginmangle-uimangle-servicesmangle-vcenter-adapter
```

## Build Profiles <a href="#build-profiles" id="build-profiles"></a>

Different build profiles available in Mangle pom are:

### default <a href="#default" id="default"></a>

This profile builds mangle with last known good configuration of mangle-byteman-agent and does not take latest changes to mangle-byteman-root module into consideration.

To build:

### build-all <a href="#build-all" id="build-all"></a>

This profile builds mangle with latest changes of mangle-byteman-agent and is recommended for use if the Mangle java agent jar has to be updated.

To build using this profile:

```
mvn clean install –P build-allORmvn clean install --activate-profiles build-all
```

### byteman <a href="#byteman" id="byteman"></a>

This profile builds only the mangle-byteman-root module with the latest changes.

To build using profile byteman

```
mvn clean install –P bytemanORmvn clean install --activate-profiles byteman
```

### vcenter-adapter <a href="#vcenter-adapter" id="vcenter-adapter"></a>

This profile builds only mangle-vcenter-adapter with the latest changes.

```
mvn clean install –P vcenter-adapterORmvn clean install --activate-profiles vcenter-adapter
```

## Building the code <a href="#building-the-code" id="building-the-code"></a>

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

#### Java <a href="#java" id="java"></a>

* Java 8 JDK installed on your OS of choice (Mac OSX, Linux variants, Windows are all supported hosts)
* ​[Eclipse Luna](http://eclipse.org) or a modern IDE of your choice. Make sure to apply the same formatting profile for code.
* Git for source code management.

#### Maven <a href="#maven" id="maven"></a>

Maven is used to build and test the project. Install maven 3.5.X to your local system.

* _(Optional)_ Install Maven with your system's package manager (e.g. _apt_ on Ubuntu/Debian, _homebrew_ on OSX, ...).
* Set your `JAVA_HOME` environment variable to be the home of the Java 8 JDK. On OSX, this lands in `/Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home/`.
* Run `mvn clean install` to compile the code, run checkstyle and run unit tests.

#### &#x20;Packaging a fat jar <a href="#packaging-a-fat-jar" id="packaging-a-fat-jar"></a>

Resulting JAR goes to `mangle-services/target/mangle-services.jar`.

`./mvnw clean package -DskipTests` (packages without running tests)

Please refer to [Mangle Administrator Guide](mangle-administration/supported-deployment-models/#deploying-the-mangle-virtual-appliance) for starting the jar by providing the Supported DB\_OPTIONS and supported CLUSTER\_OPTIONS as inputs to jar execution command.

```
java –jar mangle-services-.x.x.x-jar –D...... (Db parameters are mandatory)
```

## &#x20;Code Style <a href="#code-style" id="code-style"></a>

### License <a href="#license" id="license"></a>

Each source file has to specify the license at the very top of the file:

```
/* * Copyright (c) 2016-2019 VMware, Inc. All Rights Reserved. * * This product is licensed to you under the Apache License, Version 2.0 (the "License"). * You may not use this product except in compliance with the License. * * This product may include a number of subcomponents with separate copyright notices * and license terms. Your use of these subcomponents is subject to the terms and * conditions of the subcomponent's license, as noted in the LICENSE file. */
```

### Line length <a href="#line-length" id="line-length"></a>

Set to `100`

### Import statements <a href="#import-statements" id="import-statements"></a>

The order of import statements is:

* import static `java.*`
* import static `javax.*`
* blank line
* import static all other imports
* blank line
* import static `com.vmware.*`
* blank line
* import `java.*`
* import `javax.*`
* blank line
* import all other imports
* blank line
* import `com.vmware.*`

Comments are always placed at independent line. Do not append the comment at the end of the line.

**Wrong**

```
  host.setLoggingLevel(Level.OFF);  // setting log level OFF
```

**Correct**

```
  // setting log level OFF  host.setLoggingLevel(Level.OFF);
```

### Commit message <a href="#commit-message" id="commit-message"></a>

Follow the widely used format:

*
*

**Sample:**

```
Short (50 chars or less) summary of changes​More detailed explanatory text, if necessary.  Wrap it toabout 72 characters or so.  In some contexts, the firstline is treated as the subject of an email and the rest ofthe text as the body.  The blank line separating thesummary from the body is critical (unless you omit the bodyentirely); tools like rebase can get confused if you runthe two together.​Further paragraphs come after blank lines.​  - Bullet points are okay, too​  - Typically a hyphen or asterisk is used for the bullet,    preceded by a single space, with blank lines in    between, but conventions vary here
```

* 50 char in title
* Wrap the body at 72 char or less

### Checkstyle <a href="#checkstyle" id="checkstyle"></a>

Checkstyle runs as part of maven `validate` lifecycle.

You can call it manually like `./mvnw validate` or `./mvnw checkstyle:checkstyle`.

checkstyle file: [checkstyle.xml](https://github.com/vmware/mangle/blob/master/checkstyle/checkstyle.xml)​

## IDE Settings <a href="#ide-settings" id="ide-settings"></a>

### Formatter <a href="#formatter" id="formatter"></a>

For both Eclipse and IntellJ, import contrib/eclipse-java-style.xml

#### IntelliJ <a href="#intellij" id="intellij"></a>

IntelliJ can import eclipse formatter file.

`Preference` - `Editor` - `Code Style` - `Manage` - `Import`

Import `contrib/eclipse-java-style.xml`.

### IntelliJ Specific <a href="#intellij-specific" id="intellij-specific"></a>

#### Setting java package import order <a href="#setting-java-package-import-order" id="setting-java-package-import-order"></a>

1. Update `.idea/codeStyleSettings.xml` with contrib/idea-java-style.xml
2. Restart IntelliJ

## Building Mangle Docker Images <a href="#building-mangle-docker-images" id="building-mangle-docker-images"></a>

### **Prerequisites** <a href="#prerequisites-1" id="prerequisites-1"></a>

Install docker on the developer machine following [docker installation manual](https://docs.docker.com/v17.12/manuals/). Change the working directory to the root of mangle code repository.

### **To build a docker image for Mangle** <a href="#to-build-a-docker-image-for-mangle" id="to-build-a-docker-image-for-mangle"></a>

```
#!/bin/bash#stop the existing container if it is already runningdocker rm -f $(docker ps -a | grep mangle-app | awk '{print$1}')IMAGE=`docker images | grep "^mangle-app"| awk '{print $1}'`$CONTAINER_NAME= mangle-appif [ $IMAGE = $CONTAINER_NAME ]then               echo 'removing '$CONTAINER_NAME' image ...'               docker rmi -f $CONTAINER_NAMEfi#Build Imagedocker build –f docker/Dockerfile -t $CONTAINER_NAME .IP=`ifconfig eth0 | grep "inet addr" | cut -d ':' -f 2 | cut -d ' ' -f 1`
```

### **To build a docker image for Mangle vCenter Adapter** <a href="#to-build-a-docker-image-for-mangle-vcenter-adapter" id="to-build-a-docker-image-for-mangle-vcenter-adapter"></a>

```
#!/bin/bash#stop the existing container if it is already runningdocker rm -f $(docker ps -a | grep mangle-app | awk '{print$1}')IMAGE=`docker images | grep "^mangle-app"| awk '{print $1}'`$CONTAINER_NAME= mangle-appif [ $IMAGE = $CONTAINER_NAME ]then               echo 'removing '$CONTAINER_NAME' image ...'               docker rmi -f $CONTAINER_NAMEfi#Build Imagedocker build –f docker/Dockerfile -t $CONTAINER_NAME .IP=`ifconfig eth0 | grep "inet addr" | cut -d ':' -f 2 | cut -d ' ' -f 1`
```
