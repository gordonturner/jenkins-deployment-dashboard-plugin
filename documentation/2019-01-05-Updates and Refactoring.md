2019-01-05-Updates and Refactoring
==================================

- This is an edit of the original project with the following changes:

1. Remove AWS dashboard functionality
2. Update to include support for Nexus 2.14.3-02

- Original documentation:

https://plugins.jenkins.io/ec2-deployment-dashboard
https://wiki.jenkins.io/display/JENKINS/EC2+Deployment+Dashboard+Plugin

- Started from maven with:

```
mvn -Dmaven.test.skip=true -DskipTests=true clean hpi:run
```

- Started successfully, but way out of date

- Started release of Jenkins 1.570
- Current release of Jenkins is 2.156
- Testing locally release of Jenkins 2.150


- Attempting to update org.jenkins-ci.plugins, plugin from 1.570
https://repo.jenkins-ci.org/webapp/#/artifacts/browse/tree/General/releases/org/jenkins-ci/plugins/plugin/3.32/plugin-3.32.pom
- Latest version is 3.32
- Downloads jenkins-war-2.60.1.war

- Issue with `maven-enforcer-plugin` during build:

```
Failed to execute goal org.apache.maven.plugins:maven-enforcer-plugin:3.0.0-M2:enforce (display-info) on project ec2-deployment-dashboard: Execution display-info of goal org.apache.maven.plugins:maven-enforcer-plugin:3.0.0-M2:enforce failed: Unknown JDK version given. Should be something like "1.7", "8", "11", "12" -> [Help 1]
```

- Replaced (and references):

```
		<compileSource>1.7</compileSource>
		<compileTarget>1.7</compileTarget>
```

- With:

```
		<java.level>8</java.level>
```

- Issue with `maven-enforcer-plugin` during build, added bunch of dependencies

- On successful start, version 2.60.1 is running
- This is still a very old version
- Looking at details for upload, it was uploaded on 21-12-18
https://repo.jenkins-ci.org/webapp/#/artifacts/browse/tree/General/releases/org/jenkins-ci/plugins/plugin/3.32/plugin-3.32.pom

- Added:

```
		<jenkins.version>2.150.1</jenkins.version>
```







