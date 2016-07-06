### Hudson - Installation version 3.0.0-RC3 (Standalone and Weblogic)

About

Installation of Hudson in:

    as a standalone application
    or in a Weblogic java container

Articles Related

    Hudson - Ant Build Steps with Junit Test Call
    Hudson

Steps
Download the war

Download the Hudson.war
Type Installation
Standalone

Hudson can run in a standalone mode. To run it on port 8989 for instance:

java -jar hudson.war --httpPort=80

C:\hudson>java -jar hudson.war --httpPort=80
Running from: C:\hudson\hudson.war
[Winstone 2012/10/11 19:38:24] - Beginning extraction from war file
Oct 11, 2012 7:38:55 PM hudson.WebAppMain contextInitialized
INFO: Home directory: C:\Users\gerard\.hudson
Oct 11, 2012 7:38:56 PM hudson.util.CharacterEncodingFilter init
INFO: CharacterEncodingFilter initialized. DISABLE_FILTER: false FORCE_ENCODING: false
Using one-time self-signed certificate
[Winstone 2012/10/11 19:38:56] - HTTP Listener started: port=80
[Winstone 2012/10/11 19:38:56] - AJP13 Listener started: port=8009
Oct 11, 2012 7:38:56 PM org.hudsonci.inject.internal.SmoothieContainerBootstrap bootstrap
INFO: Bootstrapping Smoothie
[Winstone 2012/10/11 19:38:56] - Winstone Servlet Engine v0.9.10 running: controlPort=disabled
Oct 11, 2012 7:38:58 PM hudson.PluginManager createPluginStrategy
INFO: Plugin strategy: org.hudsonci.inject.internal.plugin.DelegatingPluginStrategy
Oct 11, 2012 7:38:58 PM hudson.model.Hudson$5 onAttained
INFO: Started initialization
Oct 11, 2012 7:39:35 PM hudson.model.Hudson$5 onAttained
INFO: Listed all plugins
Oct 11, 2012 7:39:38 PM hudson.model.Hudson$5 onAttained
INFO: Prepared all plugins
Oct 11, 2012 7:39:38 PM hudson.model.Hudson$5 onAttained
INFO: Started all plugins
Oct 11, 2012 7:39:38 PM hudson.model.Hudson$5 onAttained
INFO: Augmented all extensions
Oct 11, 2012 7:39:38 PM hudson.model.Hudson$5 onAttained
INFO: Loaded all jobs
Oct 11, 2012 7:39:38 PM hudson.model.Hudson$5 onAttained
INFO: Completed initialization
Oct 11, 2012 7:39:38 PM hudson.TcpSlaveAgentListener <init>
INFO: JNLP slave agent listener started on TCP port 41351
Oct 11, 2012 7:39:47 PM com.sun.jersey.server.impl.application.WebApplicationImpl _initiate
INFO: Initiating Jersey application, version 'Jersey: 1.5 01/14/2011 12:36 PM'
Oct 11, 2012 7:39:48 PM com.sun.jersey.server.impl.application.WebApplicationImpl _initiate
INFO: Initiating Jersey application, version 'Jersey: 1.5 01/14/2011 12:36 PM'
Oct 11, 2012 7:39:48 PM com.sun.jersey.server.impl.application.WebApplicationImpl _initiate
INFO: Initiating Jersey application, version 'Jersey: 1.5 01/14/2011 12:36 PM'
Oct 11, 2012 7:39:48 PM com.sun.jersey.server.impl.application.WebApplicationImpl _initiate
INFO: Initiating Jersey application, version 'Jersey: 1.5 01/14/2011 12:36 PM'
Oct 11, 2012 7:39:49 PM com.sun.jersey.server.impl.application.WebApplicationImpl _initiate
INFO: Initiating Jersey application, version 'Jersey: 1.5 01/14/2011 12:36 PM'
Oct 11, 2012 7:39:50 PM org.hudsonci.maven.plugin.install.SlaveBundleInstaller consume
INFO: Maven slave bundle installed
Oct 11, 2012 7:39:50 PM org.hudsonci.rest.plugin.RestPlugin enable
INFO: API provider JAX-RS (Jersey) enabled
Oct 11, 2012 7:39:50 PM org.hudsonci.rest.plugin.RestPlugin enable
INFO: API provider Bayeux (CometD) enabled
Oct 11, 2012 7:39:50 PM org.hudsonci.events.ready.ReadyDetector run
INFO: Hudson is ready.

Weblogic

You can deploy it in a container of an application server. This sections shows how to deploy Hudson in a Weblogic Container (version 10.3.5.0).

    Create the deployment file: application.xml

<application xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee  http://java.sun.com/xml/ns/javaee/application_5.xsd"
             version="5">
  <module id="hudson">
    <web>
     <web-uri>hudson.war</web-uri>
     <context-root>/hudson</context-root>
    </web> 
  </module>
</application>

    Create the deployment file: weblogic-application.xml

<?xml version="1.0" encoding="UTF-8"?>
<wls:weblogic-application 
     xmlns:wls="http://www.bea.com/ns/weblogic/weblogic-application"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/javaee_5.xsd
                         http://www.bea.com/ns/weblogic/weblogic-application 
                         http://www.bea.com/ns/weblogic/weblogic-application/1.0/weblogic-application.xsd">
 
  <!-- server-version: 10.3 -->
  <wls:application-param>
    <wls:param-name>webapp.encoding.default</wls:param-name>
    <wls:param-value>UTF-8</wls:param-value>
  </wls:application-param>
 
  <wls:prefer-application-packages>
   <wls:package-name>org.apache.*</wls:package-name>
   <wls:package-name>javax.xml.stream.*</wls:package-name>
   <wls:package-name>org.dom4j.*</wls:package-name>
  </wls:prefer-application-packages>
</wls:weblogic-application>

    Create the EAR file

Create the following structure:

hudson.war
META-INF/application.xml
META-INF/weblogic-application.xml

    And create an EAR archive with the Jar Utility:

jar -cvf hudson.ear *.*

added manifest
adding: hudson.war(in = 26862446) (out= 26574476)(deflated 1%)
ignoring entry META-INF/
adding: META-INF/application.xml(in = 461) (out= 206)(deflated 55%)
adding: META-INF/weblogic-application.xml(in = 962) (out= 340)(deflated 64%)

    Deploy on Weblogic (Click, click, click)

Access hudson

    Standalone: http://serverName:port/
    Weblogic: http://serverName:port/hudson

Documentation / Reference

    Installing Hudson on WebLogic Server
    Using Oracle WebLogic as a Container
    weblogic.xml Deployment Descriptor Elements
