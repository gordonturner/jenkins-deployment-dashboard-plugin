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


Errors Validating Nexus Repo
----------------------------

- During `Configure Deployment Dashboard Repository`

- On Jenkins default home page, left nav, click `Manage Jenkins`
- On Jenkins `Manage Jenkins` screen, click `Configure System`
- On Jenkins `Configure System`, find `Deployment Dashboard`
- Set `Choose Repository Type` to `Sonatype Nexus`
 
- Set details as appropriate for the artifact to deploy

- Example:
  - Set `Repository URI` to `http://builder.localdomain:8081/nexus`
  - Set `Username` to `builder`
  - Set `Password` to `XXXX`

- Testing throws error:

```
Jan 06, 2019 1:43:09 PM de.codecentric.jenkins.dashboard.DashboardViewDescriptor doTestRepositoryConnection
INFO: Verify Repository connection for URI http://builder.localdomain:8081/nexus
Jan 06, 2019 1:43:09 PM de.codecentric.jenkins.dashboard.impl.repositories.artifactory.ArtifactoryConnector <init>
INFO: ArtifactoryConnector
Jan 06, 2019 1:43:09 PM de.codecentric.jenkins.dashboard.impl.repositories.artifactory.ArtifactoryConnector canConnect
INFO: Checking Artifactory connection
[WARNING] Error while serving http://localhost:8080/jenkins/descriptorByName/de.codecentric.jenkins.dashboard.DashboardView/testRepositoryConnection
java.lang.reflect.InvocationTargetException
	at org.kohsuke.stapler.Function$MethodFunction.invoke(Function.java:400)
	at org.kohsuke.stapler.Function$InstanceFunction.invoke(Function.java:408)
	at org.kohsuke.stapler.Function.bindAndInvoke(Function.java:212)
	at org.kohsuke.stapler.Function.bindAndInvokeAndServeResponse(Function.java:145)
	at org.kohsuke.stapler.MetaClass$11.doDispatch(MetaClass.java:537)
	at org.kohsuke.stapler.NameBasedDispatcher.dispatch(NameBasedDispatcher.java:58)
	at org.kohsuke.stapler.Stapler.tryInvoke(Stapler.java:739)
	at org.kohsuke.stapler.Stapler.invoke(Stapler.java:870)
	at org.kohsuke.stapler.MetaClass$4.doDispatch(MetaClass.java:282)
	at org.kohsuke.stapler.NameBasedDispatcher.dispatch(NameBasedDispatcher.java:58)
	at org.kohsuke.stapler.Stapler.tryInvoke(Stapler.java:739)
	at org.kohsuke.stapler.Stapler.invoke(Stapler.java:870)
	at org.kohsuke.stapler.Stapler.invoke(Stapler.java:668)
	at org.kohsuke.stapler.Stapler.service(Stapler.java:238)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:790)
	at org.eclipse.jetty.servlet.ServletHolder.handle(ServletHolder.java:841)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1650)
	at hudson.util.PluginServletFilter$1.doFilter(PluginServletFilter.java:154)
	at jenkins.telemetry.impl.UserLanguages$AcceptLanguageFilter.doFilter(UserLanguages.java:128)
	at hudson.util.PluginServletFilter$1.doFilter(PluginServletFilter.java:151)
	at hudson.util.PluginServletFilter.doFilter(PluginServletFilter.java:157)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1637)
	at hudson.security.csrf.CrumbFilter.doFilter(CrumbFilter.java:64)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1637)
	at hudson.security.ChainedServletFilter$1.doFilter(ChainedServletFilter.java:84)
	at hudson.security.ChainedServletFilter.doFilter(ChainedServletFilter.java:90)
	at hudson.security.HudsonFilter.doFilter(HudsonFilter.java:171)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1637)
	at org.kohsuke.stapler.compression.CompressionFilter.doFilter(CompressionFilter.java:49)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1637)
	at hudson.util.CharacterEncodingFilter.doFilter(CharacterEncodingFilter.java:82)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1637)
	at org.kohsuke.stapler.DiagnosticThreadNameFilter.doFilter(DiagnosticThreadNameFilter.java:30)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1637)
	at org.eclipse.jetty.servlet.ServletHandler.doHandle(ServletHandler.java:533)
	at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:143)
	at org.eclipse.jetty.security.SecurityHandler.handle(SecurityHandler.java:524)
	at org.eclipse.jetty.server.handler.HandlerWrapper.handle(HandlerWrapper.java:132)
	at org.eclipse.jetty.server.handler.ScopedHandler.nextHandle(ScopedHandler.java:190)
	at org.eclipse.jetty.server.session.SessionHandler.doHandle(SessionHandler.java:1595)
	at org.eclipse.jetty.server.handler.ScopedHandler.nextHandle(ScopedHandler.java:188)
	at org.eclipse.jetty.server.handler.ContextHandler.doHandle(ContextHandler.java:1253)
	at org.eclipse.jetty.server.handler.ScopedHandler.nextScope(ScopedHandler.java:168)
	at org.eclipse.jetty.servlet.ServletHandler.doScope(ServletHandler.java:473)
	at org.eclipse.jetty.server.session.SessionHandler.doScope(SessionHandler.java:1564)
	at org.eclipse.jetty.server.handler.ScopedHandler.nextScope(ScopedHandler.java:166)
	at org.eclipse.jetty.server.handler.ContextHandler.doScope(ContextHandler.java:1155)
	at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:141)
	at org.eclipse.jetty.server.handler.ContextHandlerCollection.handle(ContextHandlerCollection.java:219)
	at org.eclipse.jetty.server.handler.HandlerCollection.handle(HandlerCollection.java:126)
	at org.eclipse.jetty.server.handler.HandlerWrapper.handle(HandlerWrapper.java:132)
	at org.eclipse.jetty.server.Server.handle(Server.java:564)
	at org.eclipse.jetty.server.HttpChannel.handle(HttpChannel.java:317)
	at org.eclipse.jetty.server.HttpConnection.onFillable(HttpConnection.java:251)
	at org.eclipse.jetty.io.AbstractConnection$ReadCallback.succeeded(AbstractConnection.java:279)
	at org.eclipse.jetty.io.FillInterest.fillable(FillInterest.java:110)
	at org.eclipse.jetty.io.ChannelEndPoint$2.run(ChannelEndPoint.java:124)
	at org.eclipse.jetty.util.thread.Invocable.invokePreferred(Invocable.java:128)
	at org.eclipse.jetty.util.thread.Invocable$InvocableExecutor.invoke(Invocable.java:222)
	at org.eclipse.jetty.util.thread.strategy.EatWhatYouKill.doProduce(EatWhatYouKill.java:294)
	at org.eclipse.jetty.util.thread.strategy.EatWhatYouKill.run(EatWhatYouKill.java:199)
	at org.eclipse.jetty.util.thread.QueuedThreadPool.runJob(QueuedThreadPool.java:672)
	at org.eclipse.jetty.util.thread.QueuedThreadPool$2.run(QueuedThreadPool.java:590)
	at java.lang.Thread.run(Thread.java:745)
Caused by: java.lang.NoClassDefFoundError: Could not initialize class com.sun.jersey.core.header.MediaTypes
	at com.sun.jersey.core.spi.factory.MessageBodyFactory.initReaders(MessageBodyFactory.java:182)
	at com.sun.jersey.core.spi.factory.MessageBodyFactory.initReaders(MessageBodyFactory.java:176)
	at com.sun.jersey.core.spi.factory.MessageBodyFactory.init(MessageBodyFactory.java:162)
	at com.sun.jersey.api.client.Client.init(Client.java:342)
	at com.sun.jersey.api.client.Client.access$000(Client.java:118)
	at com.sun.jersey.api.client.Client$1.f(Client.java:191)
	at com.sun.jersey.api.client.Client$1.f(Client.java:187)
	at com.sun.jersey.spi.inject.Errors.processWithErrors(Errors.java:193)
	at com.sun.jersey.api.client.Client.<init>(Client.java:187)
	at com.sun.jersey.client.apache.ApacheHttpClient.<init>(ApacheHttpClient.java:137)
	at com.sun.jersey.client.apache.ApacheHttpClient.create(ApacheHttpClient.java:179)
	at com.sun.jersey.client.apache.ApacheHttpClient.create(ApacheHttpClient.java:168)
	at de.codecentric.jenkins.dashboard.impl.repositories.artifactory.ArtifactoryConnector.buildClient(ArtifactoryConnector.java:124)
	at de.codecentric.jenkins.dashboard.impl.repositories.artifactory.ArtifactoryConnector.getResponse(ArtifactoryConnector.java:114)
	at de.codecentric.jenkins.dashboard.impl.repositories.artifactory.ArtifactoryConnector.canConnect(ArtifactoryConnector.java:53)
	at de.codecentric.jenkins.dashboard.DashboardViewDescriptor.doTestRepositoryConnection(DashboardViewDescriptor.java:92)
	at java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:627)
	at org.kohsuke.stapler.Function$MethodFunction.invoke(Function.java:396)
	... 63 more

```


Recommended Plugins for 2.150.1
-------------------------------

Folders 
OWASP Markup Formatter 
Build Timeout 
Credentials Binding 
Timestamper 
Workspace Cleanup 
Ant 
Gradle 
Pipeline 
GitHub Branch Source 
Pipeline: GitHub Groovy Libraries 
Pipeline: Stage View
Git
Subversion
SSH Slaves 
Matrix Authorization Strategy 
PAM Authentication 
LDAP 
Email Extension 
Mailer













