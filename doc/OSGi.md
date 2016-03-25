![OSGi](https://www.osgi.org/wp-content/uploads/logo.jpg)

The OSGi (Open Services Gateway initiative), is an open standards organization founded in March 1999 that originally specified and maintains the OSGi standard. 

OSGi's vision is the emergence of applications from assembling (dynamically) different reusable components that have no a-priori knowledge of each other.

#OSGi technology

The OSGi technology is a set of specifications that define a dynamic component system for Java. These specifications enable a development model where applications are (dynamically) composed of many different (reusable) components. 

The OSGi specification describes a modular system and a service platform for the Java programming language that implements a complete and dynamic component model (does not exist in standalone Java/VM environments). Applications or components, coming in the form of bundles for deployment, can be remotely installed, started, stopped, updated, and uninstalled without requiring a reboot. Application life cycle management is implemented via APIs that allow for remote downloading of management policies. The service registry allows bundles to detect the addition of new services, or the removal of services, and adapt accordingly.

The OSGi specifications have evolved beyond the original focus of service gateways, and are now used in applications ranging from mobile phones to the open-source Eclipse IDE. Other application areas include automobiles, industrial automation, building automation, PDAs, grid computing, entertainment, fleet management and application servers.

The OSGi specification is developed by the members in an open process and made available to the public free of charge under the OSGI Sepcification License <https://www.osgi.org/Main/OSGiSpecificationLicense>.

#OSGi architecture
Any framework that implements the OSGi standard provides an environment for the modularization of applications into smaller bundles. Each bundle is a tightly coupled, dynamically loadable collection of classes, jars, and configuration files that explicitly declare their external dependencies.

The framework is conceptually divided into the following areas:

![OSGi system layering](http://ghptech.github.io/doc/Osgi_framework.svg)

* **Bundles/Plug-ins**  are OSGi (normal JARs) components with extra manifest headers (made by developers).
* **Services** layer connects bundles in a dynamic way by offering a publish-find-bind model for Plain Old Java Interfaces (POJI) or Plain Old Java Objects (POJO).
* **Life-Cycle** is the API (Application Programming Interface) employed to install, start, stop, update, and uninstall bundles.
* **Modules** are layers that defines how a bundle can import and export code.
* **Security** is a layer that handles the security aspects.
* **Execution Environment** defines what methods and classes are available in a specific platform.

##Bundles

![OSGi system layering](http://ghptech.github.io/doc/Osgi-system-layering.svg)

A bundle is a group of Java classes and additional resources equipped with a detailed manifest MANIFEST.MF file on all its contents, as well as additional services needed to give the included group of Java classes more sophisticated behaviors, to the extent of deeming the entire aggregate a component.
Below is an example of a typical MANIFEST.MF file with OSGi Headers:

    Bundle-Name: Hello World
    Bundle-SymbolicName: org.wikipedia.helloworld
    Bundle-Description: A Hello World bundle
    Bundle-ManifestVersion: 2
    Bundle-Version: 1.0.0
    Bundle-Activator: org.wikipedia.Activator
    Export-Package: org.wikipedia.helloworld;version="1.0.0"
    Import-Package: org.osgi.framework;version="1.3.0"  
    
The meaning of the contents in the example is as follows:

* Bundle-Name: Defines a human-readable name for this bundle, Simply assigns a short name to the bundle.
* Bundle-SymbolicName: The only required header, this entry specifies a unique identifier for a bundle, based on the reverse domain name convention (used also by the java packages).
* Bundle-Description: A description of the bundle's functionality.
* Bundle-ManifestVersion: Indicates the OSGi specification to use for reading this bundle.
* Bundle-Version: Designates a version number to the bundle.
* Bundle-Activator: Indicates the class name to be invoked once a bundle is activated.
* Export-Package: Expresses which Java packages contained in a bundle will be made available to the outside world.
* Import-Package: Indicates which Java packages will be required from the outside world to fulfill the dependencies needed in a bundle.

##Life Cycle

![OSGi system layering](http://ghptech.github.io/doc/OSGi_Bundle_Life-Cycle.svg)

A Life Cycle layer adds bundles that can be dynamically installed, started, stopped, updated and uninstalled. Bundles rely on the module layer for class loading but add an API to manage the modules in run time. The life cycle layer introduces dynamics that are normally not part of an application. Extensive dependency mechanisms are used to assure the correct operation of the environment. Life cycle operations are fully protected with the security architecture.

<table>
    <tr>
        <td>Bundle State</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>INSTALLED</td>
        <td>The bundle has been successfully installed</td>
    </tr>
    <tr>
        <td>RESOLVED</td>        
        <td>All Java classes that the bundle needs are available. This state indicates that the bundle is either ready to be started or has stopped</td>
    </tr>
    <tr>
        <td>STARTING</td>	    
        <td>The bundle is being started, the BundleActivator.start method has been called but the start method has not yet returned. When the bundle has an activation policy, the bundle will remain in the STARTING state until the bundle is activated according to its activation policy</td>
    </tr>
    <tr>
        <td>ACTIVE</td>	        
        <td>The bundle has been successfully activated and is running; its Bundle Activator start method has been called and returned</td>
    </tr>
    <tr>
        <td>STOPPING</td>
        <td>The bundle is being stopped. The BundleActivator.stop method has been called but the stop method has not yet returned</td>
    </tr>
    <tr>
        <td>UNINSTALLED</td>     
        <td>The bundle has been uninstalled. It cannot move into another state</td>
    </tr>
</table>

Below is an example of a typical Java class implementing the BundleActivator interface:

    package org.wikipedia;
    import org.osgi.framework.BundleActivator;
    import org.osgi.framework.BundleContext;
    public class Activator implements BundleActivator {
        private BundleContext context;
        @Override
        public void start(BundleContext context) throws Exception {
            System.out.println("Starting: Hello World");
            this.context = context;
            }
        @Override
        public void stop(BundleContext context) throws Exception {
            System.out.println("Stopping: Goodbye Cruel World");
            this.context = null;
            }
    }

##Modules
Modularity is at the core of the OSGi specifications and embodied in the bundle concept. In Java terms, a bundle is a plain JAR file. However, where in standard Java everything in a JAR is completely visible to all other JARs, OSGi hides everything in that JAR unless explicitly exported. A bundle that wants to use another JAR must explicitly import the parts it needs (no sharing by default).

Though the code hiding and explicit sharing provides many benefits (for example, allowing multiple versions of the same library being used in a single VM), the code sharing was only there to support OSGi services model. The services model is about bundles that collaborate.

##Services

A bundle can create an object and register it with the OSGi service registry under one or more interfaces. Other bundles can go to the registry and list all objects that are registered under a specific interfaces or class. For example, a bundle provides an implementation of the DocumentBuilder. When it gets started, it creates an instance of its DocumentBuilderFactoryImpl class and registers it with the registry under the DocumentBuilderFactory class. A bundle that needs a DocumentBuilderFactory can go to the registry and ask for all available services that extend the DocumentBuilderFactory class. Even better, a bundle can wait for a specific service to appear and then get a call back.

A bundle can register a service, it can get a service, and it can listen for a service to appear or disappear. Any number of bundles can register the same service type, and any number of bundles can get the same service. 

![OSGi framework](https://raw.githubusercontent.com/ghpopovici/openEMS/master/doc/OSGi_services.png)

Services are dynamic. This means that a bundle can decide to withdraw its service from the registry while other bundles are still using this service. Bundles using such a service must then ensure that they no longer use the service object and drop any references.

This might seam like a significant complexity but it turns out that helper classes like the Service Tracker and frameworks like iPOJO, Spring, and Declarative Services can make the pain minimal while the advantages are quite large. The service dynamics were added so installation and uninstallation of bundles on the fly while the other bundles could adapt is possible. A bundle could still provide functionality even if the http service went away. However, we found out over time that the real world is dynamic and many problems are a lot easier to model with dynamic services than static factories. For example, a Device service could represent a device on the local network. If the device goes away, the service representing it is unregistered. This way, the availability of the service models the availability of a real world entity. This works out very well in, for example, the distributed OSGi model where a service can be withdrawn if the connection to the remote machine is gone. It also turns out that the dynamics solve the initialization problem. OSGi applications do not require a specific start ordering in their bundles.

The effect of the service registry has been that many specialized APIs can be much modeled with the service registry. Not only does this simplify the overall application, it also means that standard tools can be used to debug and see how the system is wired up.

Though the service registry accepts any object as a service, the best way to achieve reuse is to register these objects under (standard) interfaces to decouple the implementer from the client code. This is the reason the OSGi Alliance publishes the Compendium specifications. These specification define a large number of standard services, from a Log Service to a Measurement and State specification. All these standardized services are described in great detail.

##Deployment

Bundles are deployed on an OSGi framework, the bundle runtime environment. This is not a container like Java Application Servers. It is a collaborative environment. Bundles run in the same VM and can actually share code. The framework uses the explicit imports and exports to wire up the bundles so they do not have to concern themselves with class loading. Another contrast with the application servers is that the management of the framework is standardized. A simple API allows bundles to install, start, stop, and update other bundles, as well as enumerating the bundles and their service usage. This API has been used by many management agents to control OSGi frameworks. Management agents are as diverse as the Knopflerfish desktop and an IBM Tivoli management server.



